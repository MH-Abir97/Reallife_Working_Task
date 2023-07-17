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

