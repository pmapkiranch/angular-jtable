//directive begin

    var angularjtable = angular.module('angularjtable', []);
    
    angularjtable.filter('toArray', function () {
    return function (obj, addKey) {
        if (!(obj instanceof Object)) {
            return obj;
        }

        if (addKey === false) {
            return Object.values(obj);
        } else {
            return Object.keys(obj).map(function (key) {
                return Object.defineProperty(obj[key], '$key', { enumerable: false, value: key });
            });
        }
    };
});

angularjtable.filter('range', function () {
    return function (input, total) {
        total = parseInt(total);

        for (var i = 1; i <= total; i++) {
            input.push(i);
        }

        return input;
    };
});


angularjtable.directive('escKey', function () {
    return function (scope, element, attrs) {
        element.bind('keydown keypress', function (event) {
            if (event.which === 27) { // 27 = esc key
                scope.$apply(function () {
                    scope.$eval(attrs.escKey);
                });

                event.preventDefault();
            }
        });
    };
})


    angularjtable.directive('clearable', function ($compile, $timeout) {
    return {
        require: 'ngModel',
        link: function (scope, elem, attrs, ctrl) {
            elem.addClass('clearable');

            elem.bind('input', function () {
                elem[toggleClass(elem.val())]('x');
            });

            elem.on('mousemove', function (e) {
                if (elem.hasClass('x')) {
                    elem[toggleClass(this.offsetWidth - 25 < e.clientX - this.getBoundingClientRect().left)]('onX');
                }
            });

            elem.on('click', function (e) {
                if (elem.hasClass('onX')) {
                    elem.removeClass('x onX').val(undefined);
                    ctrl.$setViewValue(undefined);
                    ctrl.$render();
                    scope.$digest();
                }
            });

            function toggleClass(v) {
                return v ? 'addClass' : 'removeClass';
            }
        }
    };
});

angularjtable.directive('dynamic', function ($compile) {
    return {
        restrict: 'A',
        replace: true,
        link: function (scope, ele, attrs) {
            if (attrs.dynamic != "") {
                var template = $compile(attrs.dynamic)(scope);
                ele.replaceWith(template);
            }


        }
    };
});

  angularjtable.directive('jtable', function () {
    return {
        restrict: 'E',
        scope: {
            items: '=',
            selecteditems:'=?',
            options: '=',
            parentItem: '=?',
            vm: '=?'
        },
        controller: function($scope,$filter){
          
          if (!$scope.options) {
        console.warn('angular jtable options not passed');
        return;
    }

    if (!$scope.options.fields) {
        console.warn('angular jtable fields not passed');
        return;
    }

    if ($scope.options.fields.children) {
        $scope.options.children = $scope.options.fields.children;
        delete $scope.options.fields.children;
    }


    if (!$scope.items)
        $scope.items = [];

    if (!$scope.selecteditems)
        $scope.selecteditems = [];

    $scope.pager = {};

    var PagerService = getPagerService();
   

    $scope.expandAll = function (items, flag) {
        angular.forEach(items, function (row, index) {
            row.isExpanded = flag;
            if (row.items && row.items.length > 0)
                $scope.expandAll(row.items, flag);
        });

    }

    initController();

    function initController() {

        if (!$scope.options.pageSize)
            $scope.options.pageSize = 10;

       
        $scope.setPage = setPage;
        $scope.setPage(1);

    }
    
    //#region events
    $scope.filterTable = function () {
        $scope.filteredItems = $filter('filter')($scope.items, $scope.model.filter);
        $scope.setPage($scope.pager.currentPage, $scope.filteredItems);
    }

    
    $scope.selectAll = function (items, flag) {

        angular.forEach(items, function (row, index) {
            row.isSelected = flag;

        });

    }
    $scope.selectItem = function (item) {

        if (item.isSelected)
            $scope.selecteditems.push(item);
        else {
            var index = $scope.filteredItems.indexOf(item);
            $scope.selecteditems.splice(index, 1);

        }

        var allSelected = true;
        angular.forEach($scope.filteredItems, function (row, index) {
            if (allSelected) {
                if (!row.isSelected) {
                    allSelected = false;
                }
            }

        });

        $scope.model.IsAllSelected = allSelected;
    }



    $scope.$watch('model.filter', function (newNames, oldNames) {
        if ($scope.model.filter === "")
            $scope.filterTable();
    });

    $scope.$watchCollection('items', function (newcollection, oldcollection) {
        $scope.filteredItems = newcollection;
        $scope.setPage($scope.pager.currentPage, $scope.filteredItems);
    });


    $scope.getNofPages = function (items, pageSize) {
        if (!items)
            return 1;

        var result = Math.round(items.length / pageSize);
        return result <= 0 ? 1 : result;
    }


    $scope.editItem = function (editAction, item) {

        if (typeof editAction === "function") {
            editAction(item);
        }


    }


    $scope.deleteItem = function (deleteAction, item) {

        if (typeof deleteAction === "function")
            deleteAction(item);


    }
    $scope.addItem = function (addAction, item) {

        if (typeof addAction === "function")
            addAction(item);

    }

    $scope.sortData = function (header) {
        header.isASC = !header.isASC;
        header.isDESC = !header.isASC;
        if (!$scope.options.sorting)
            return;

        var columnName = header.title;
        if (header.isASC) {

            $scope.filteredItems = Enumerable.From($scope.filteredItems).OrderBy(function (x) { return x[columnName] }).ToArray();
        }
        else {


            $scope.filteredItems = Enumerable.From($scope.filteredItems).OrderByDescending(function (x) { return x[columnName] }).ToArray();

        }

    }

    $scope.getPreviousRowCount = function () {
        return ($scope.pager.currentPage - 1) * $scope.options.pageSize;
    }
    $scope.options.columns = Object.getOwnPropertyNames($scope.options.fields);

    $scope.getAllColumnCount = function () {
        var count = $scope.options.columns.length;
        if ($scope.options.actions && $scope.options.actions.deleteAction)
            count++;

        if ($scope.options.actions && $scope.options.actions.editAction)
            count++;
        return count + 1;
    }
    //#endregion

    //#region private functions
    function getPagerService() {
        // service definition
        var service = {
        };

        service.GetPager = GetPager;

        return service;

        // service implementation
        function GetPager(totalItems, currentPage, pageSize) {
            // default to first page
            currentPage = currentPage || 1;

            // default page size is 10
            pageSize = pageSize || 10;

            // calculate total pages
            var totalPages = Math.ceil(totalItems / pageSize);

            var startPage, endPage;
            if (totalPages <= 10) {
                // less than 10 total pages so show all
                startPage = 1;
                endPage = totalPages;
            } else {
                // more than 10 total pages so calculate start and end pages
                if (currentPage <= 6) {
                    startPage = 1;
                    endPage = 10;
                } else if (currentPage + 4 >= totalPages) {
                    startPage = totalPages - 9;
                    endPage = totalPages;
                } else {
                    startPage = currentPage - 5;
                    endPage = currentPage + 4;
                }
            }

            // calculate start and end item indexes
            var startIndex = (currentPage - 1) * pageSize;
            var endIndex = Math.min(startIndex + pageSize - 1, totalItems - 1);

            // create an array of pages to ng-repeat in the pager control
            var pages = _.range(startPage, endPage + 1);

            // return object with all pager properties required by the view
            return {
                totalItems: totalItems,
                currentPage: currentPage,
                pageSize: pageSize,
                totalPages: totalPages,
                startPage: startPage,
                endPage: endPage,
                startIndex: startIndex,
                endIndex: endIndex,
                pages: pages
            };
        }
    }





    function setPage(page, items) {

        if (!$scope.options.paging) {
            $scope.filteredItems = $scope.items;
            return;
        }

        if (!items)
            items = $scope.items;


        if (items === null || items.length <= 0 || page < 1 || page > $scope.pager.totalPages) {
            return;
        }




        // get pager object from service
        $scope.pager = PagerService.GetPager(items.length, page, $scope.options.pageSize);

        // get current page of items

        $scope.filteredItems = items.slice($scope.pager.startIndex, $scope.pager.endIndex + 1);
        $scope.expandAll($scope.filteredItems, false);
    }
    //#endregion
    console.log("jtable options passed :");
    console.log($scope.options);
          
        },
        link: function (scope, element, attrs, controllers) {

        }

        ,

        templateUrl:"angular-jtable-tpl.html",
    };
});
  
