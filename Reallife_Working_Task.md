# Working Task
# Distinct Data
```
   $scope.SupplierBillList = data.filter((Sup, aData, index) => index.findIndex(v => v.SupplierName === Sup.SupplierName) === aData);
````
   # List Merge with Plus

   ```
 $scope.inv_JumboStockIssueDetailList = $scope.inv_JumboStockIssueDetailList.reduce((r, { DepartmentId, IssuedJumboRollQty, IssuedRawMatQty, IssuedRawMatUnitPrice, JumboItemName, JumboWastageInMM, MaterialTypeId, RawItemName, RawItemId, RawItemUnitId, RawRollWidthInMeter, RawRollLenghtInMeter, JumboRollWidthInMeter, 
     JumboRollLenghtInMeter, SubCategoryId }) => {
            var temp = r.find(o => o.RawItemId === RawItemId);
            if (!temp) {
                r.push({ DepartmentId, IssuedJumboRollQty, IssuedRawMatQty, IssuedRawMatUnitPrice, JumboItemName, JumboWastageInMM, MaterialTypeId, RawItemId, RawItemName, RawItemUnitId, RawRollWidthInMeter, RawRollLenghtInMeter, JumboRollWidthInMeter, JumboRollLenghtInMeter, SubCategoryId });
            } else {
                temp.IssuedRawMatQty += IssuedRawMatQty;
            }
            return r;
        }, []);
```

## Dropdown Value Color

```
 function formatOutput(optionElement) {
        //if (!optionElement.id) { return optionElement.text; }
        var ItemCombination = '';
        var DescriptionPart = optionElement.text.split('Sub Category: ');
        var SubCategoryName = DescriptionPart[1];
        if (SubCategoryName == 'Pre Printed Label') {
            ItemCombination = '<strong style="background-color: #dd4b39; color: white;">' + DescriptionPart[0] + 'Sub Category: ' + DescriptionPart[1] + '</strong>';
        } else {
            ItemCombination = DescriptionPart[0] + 'Sub Category: ' + DescriptionPart[1];
        }

        var $state = $(
            '<span>' + ItemCombination + '</span>'
        );
        return $state;
    };

 function GetByCombinationand() {
        $http({
            url: "/Item/GetAllItem",
            method: "GET",
            headers: { 'Content-Type': "application/json" }
        }).success(function (data) {
            //$scope.ItemList = data;

            angular.forEach(data, function (aData) {
            
                    aData.Combination = aData.ItemName +
                        " ~ " +
                        aData.ItemDescription +
                        " ~ " +
                        aData.ItemCode
                        +
                        " ~ " + "Sub Category: " +
                        aData.SubCategoryName;

                    $scope.ItemList.push(aData)
  



            })
        })
    }

$("#SearchTextBox").select2({

        theme: "classic",
        dropdownAutoWidth: false,
        templateResult: formatOutput,
        width: 'resolve'
    });



 

```

## List group By

```
 $scope.ProductionHistoryList = Array.from(
                $scope.ProductionHistoryList.reduce((m, o) => m.set(o.ProductionNo, (m.get(o.ProductionNo) || []).concat(o)), new Map),
                ([ProductionNo, events]) => ({ ProductionNo, events })
            );



#Cascade droupdown list
<!DOCTYPE html>
<html>
<head>
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.6.4/jquery.min.js"></script>
<script>
$(document).ready(function(){
 var list=[
    {ProductId:1,Name:"Product 1 ",CategoryId:1},
    {ProductId:2,Name:"Product 2 ",CategoryId:2}
 ];
 
  var list2=[
    {ID:1,Name:"Raw 1 ",ProductId:1},
    {ID:2,Name:"finished 2 ",ProductId:2}
 ];
 
 $.each(list, function (index, value) {
    $('#ProductId').append('<option value="' + value.ProductId + '">' + value.Name + '</option>');
  });
  
   $('#ProductId').change(function () {
      var id=$(this).val();
       $('#ProductId1').empty().append('<option value=""> -- Select Category -- </option>');
       $.each(list2, function (index, value) {
           if(value.ProductId==id){
             $('#ProductId1').append('<option value="' + value.ID + '">' + value.Name + '</option>');
           }

       });
   })
});
</script>
</head>
<body>

<button>Hide</button>

<select name="cars" id="ProductId">
  <option value="">--Select--</option>
 
</select>

<select name="cars" id="ProductId1">
<option value=""> -- Select Category -- </option>
</select>

</body>
</html>


```




