# angular-jtable
This repo is alternate solution for most popular jtable which is javascript based table which has in built sorting,paging,editing etc. But angular users were bit disappointed so I've started this project to mimic jtable. It looks exactly like jTable but it's angular table. Please see the plunker for implementation http://plnkr.co/edit/D7oics3tl2i3elowaALu?p=preview

### Prerequisites
- bootsrap

- fontawesome

- jtable css (https://github.com/hikalkan/jtable/tree/master/lib/themes/lightcolor)

- underscore (https://cdn.jsdelivr.net/underscorejs/1.8.3/underscore.js)

# Usage

### In html:
``` html

<jtable items="items" options="JtableOptions"></jtable>
```

### In your controller:
```js
// Code goes here


    var app = angular.module('myApp', ["angularjtable"]);
    
app.controller("jtableTestController", function($scope) {
  $scope.vm = {};
  var vm = $scope.vm;
  vm.message = "angular working";

  $scope.items = [{
    Id: 3,
    Name: "UK2",
    Percentage: "71"
  }, {
    Id: 4,
    Name: "UK3",
    Percentage: "71",
    items: [{
      Id: 1,
      Name: 'Hello1',
      Percentage: 1
    }, {
      Id: 2,
      Name: 'Hello2',
      Percentage: 2
    }, ]
  }];

  $scope.JtableOptions = getJtableColumns();


  function getJtableColumns() {
    var columns = {
      
      title: 'Jtable test',
      paging: true, //Enable paging
      pageSize: 2, //Set page size (default: 2)
      sorting: true, //Enable sorting
      defaultSorting: 'Name ASC',
      filter:true,
      selecting:true,
      actions: {
        editAction: function() {
          alert('i am on main edit');
        },
        addAction: function(parentItem) {
          alert('add clicked');
        },
        deleteAction: function(item) {
          alert('delete clicked');
        }
      },
      fields: {


        Name: {
          title: 'Country of Production',
          width: '70%'
        },

        Percentage: {
          width: '20%',
          title: 'Percentage(double click me)',
          OnEdit: function(data) {
            var edit = "<input  type='text' ng-model='item.Percentage' />";

            return edit;
          },
          display: function(data) {

          }
        },


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

