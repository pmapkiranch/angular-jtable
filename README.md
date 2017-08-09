# angular-jtable
This repo is alternate solution for most popular jtable which is javascript based table which has in built sorting,paging,editing etc. But angular users were bit disappointed so I've started this project to mimic jtable. It looks exactly like jTable but it's angular table. 

- For demo please see the plunker for implementation http://plnkr.co/edit/D7oics3tl2i3elowaALu?p=preview

### Prerequisites
- bootsrap

- fontawesome

- jtable css (https://github.com/hikalkan/jtable/tree/master/lib/themes/lightcolor)

- underscore (https://cdn.jsdelivr.net/underscorejs/1.8.3/underscore.js)

# Usage

### In html:
``` html

<jtable items="items" options="JtableOptions" vm="vm" selecteditems="vm.selectedrows"></jtable>
```

### In your controller:
```js
// Code goes here


   // Code goes here


    var app = angular.module('myApp', ["angularjtable"]);
    
app.controller("jtableTestController", function($scope) {
  $scope.vm = {};
  var vm = $scope.vm;
  vm.selectedrows=[];
  vm.message = "angular jtable demo working";

  $scope.items = [{
    Id: 3,
    Name: "UK",
    Percentage: "20",
     items: [{
      Id: 1,
      Name: 'London',
      Percentage: "10"
    }, {
      Id: 2,
      Name: 'Leeds',
      Percentage: "10"
    }, ]
  }, {
    Id: 4,
    Name: "USA",
    Percentage: "80",
    items: [{
      Id: 1,
      Name: 'Texas',
      Percentage: "40"
    }, {
      Id: 2,
      Name: 'Chicago',
      Percentage: "40"
    }, ]
  }];

  $scope.JtableOptions = getJtableColumns();

vm.clicktoselect=function(item){
  
  item.isSelected=!item.isSelected;
}
  function getJtableColumns() {
    var columns = {
      controller: 'jtableTestController', //This is to resolve controller methods that you used in Display,OnEdit functions
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
          ,
          display: function(data) {
 var html = "<div  ng-click=\"vm.clicktoselect(item)\">{{item.Name}}</div>";

            return html;
          }
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
        children:getChildJtableColumns(),


      },

    };

    return columns;
  }
  
  
  
  
   function getChildJtableColumns() {
    var columns = {
     
      title: 'Jtable child test',
      paging: true, //Enable paging
      pageSize: 2, //Set page size (default: 2)
      sorting: true, //Enable sorting
      defaultSorting: 'Name ASC',
      filter:true,
      selecting:true,
      actions: {
        editAction: function() {
          alert('i am on child edit');
        },
        addAction: function(parentItem) {
          alert('child add clicked');
        },
        deleteAction: function(item) {
          alert('child delete clicked');
        }
      },
      fields: {


        Name: {
          title: 'State of Production',
          width: '70%'
        },

        Percentage: {
          width: '20%',
          title: 'Percentage(double click me)',
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

