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



