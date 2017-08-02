# angular-jtable
This repo is alternate solution for most popular jtable which is javascript based table which has in built sorting,paging,editing etc. But angular users were bit disappointed so I've started this project to mimic jtable. It looks exactly like jTable but it's angular table.

### Prerequisites
- bootsrap

- fontawesome

- jtable css (https://github.com/hikalkan/jtable/tree/master/lib/themes/lightcolor)

# Usage
### In your directive folder
  - copy jtable-directive.js file
  - copy jtable-template.html file
  - Load jtable-directive.js into your index.html file or whereever you load all scripts.
  -  ``` For example <script src='\jtable-directive.js'></script>```
  
### In html:
``` html

<jtable items="items" options="JtableOptions"></jtable>
```

### In your controller:
```js
app.controller("jtableTestController", function ($rootScope, $scope, $http, $timeout, $state, $uibModal) {
 $scope.vm = {};
    var vm = $scope.vm;
    vm.addItemClick=function(parentItem){
    //TODO open popup
    }

           $scope.items =  [{ Id: 3, Name: "UK2", Percentage: "71" },
        {
            Id: 4, Name: "UK3", Percentage: "71",
            items: [{ Id: 1, Name: 'Hello1', Percentage: 1 }, { Id: 2, Name: 'Hello2', Percentage: 2 }, ]
        }];

$scope.JtableOptions = getJtableColumns();


 function getJtableColumns() {
        var columns = {
            controller: 'jtableTestController',//This is to resolve controller methods that you used in Display,OnEdit functions
            title: 'Jtable test',
            paging: true, //Enable paging
            pageSize: 2, //Set page size (default: 2)
            sorting: true, //Enable sorting
            defaultSorting: 'Name ASC',
            actions: {
                editAction: function () {
                    alert('i am on main edit')
                },
                addAction: function (parentItem) {
                    vm.addItemClick(parentItem);
                },
                deleteAction: function (item) {
                    vm.deleteItem(item);
                }
            },
            fields: {


                Name: {
                    title: 'Country of Production',
                    width: '70%'
                },

                Percentage: {
                    width: '20%',
                    title: 'Percentage',
                    OnEdit: function (data) {
                       var edit = "<input numbersOnly class='jtableTextbox numbersOnly'  type='text' ng-model='item.Percentage' />"

                       return edit;
                   }
                   ,
                    display: function (data) {
                      
                    }
                },
               children: getChildJtableColumns(),

            },

        };

        return columns;
    }

});
```


## Authors

* **Kiran Cholleti** - *Initial work* - 



## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details

## Acknowledgments
* Inspiration from https://github.com/hikalkan/jtable

