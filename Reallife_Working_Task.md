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




```

## Cascade droupdown list

```
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
```
@{
    ViewBag.Title = "About";
}
<br />
<br />
<input type="file" id="fileInput" accept=".xlsx, .xls" />
<button id="convertButton">Convert Excel to JavaScript</button>
<button id="exportButton">Download file</button>


<script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.17a.5/xlsx.full.min.js"></script>
<script>
    document.getElementById("convertButton").addEventListener("click", function () {
        const fileInput = document.getElementById("fileInput");

        if (fileInput.files.length === 0) {
            alert("Please select an Excel file.");
            return;
        }

        const selectedFile = fileInput.files[0];
        const reader = new FileReader();

        reader.onload = function (e) {
            const data = e.target.result;
            const workbook = XLSX.read(data, { type: "binary" });

            // Assuming the first sheet is the one you want to convert
            const sheetName = workbook.SheetNames[0];
            const sheet = workbook.Sheets[sheetName];

            // Convert the sheet data to an array of objects
            const result = XLSX.utils.sheet_to_json(sheet, { header: 1 });

            console.log(result);



            // Assuming the first array contains headers
            const headers = result[0];

            // Initialize an empty array to store the JSON objects
            const jsonDataArray = [];

            // Iterate through the data arrays, starting from the second array
            for (let i = 1; i < result.length; i++) {
                const data = result[i];
                const jsonObject = {};

                // Map the data to the headers
                for (let j = 0; j < headers.length; j++) {
                    jsonObject[headers[j]] = data[j];
                }

                // Push the JSON object to the array
                jsonDataArray.push(jsonObject);
            }

            // jsonDataArray now contains the data in JSON format
            console.log(JSON.stringify(jsonDataArray, null, 2));

            // jsonData is an array of objects containing your Excel data
        };

        reader.readAsBinaryString(selectedFile);
    });



    //document.getElementById("convertButton").addEventListener("click", function () {
    //    const fileInput = document.getElementById("fileInput");

    //    if (fileInput.files.length === 0) {
    //        alert("Please select an Excel file.");
    //        return;
    //    }

    //    const selectedFile = fileInput.files[0];
    //    const reader = new FileReader();

    //    reader.onload = function (e) {
    //        const data = e.target.result;
    //        const workbook = XLSX.read(data, { type: "binary" });

    //        // Assuming the first sheet is the one you want to convert
    //        const sheetName = workbook.SheetNames[0];
    //        const sheet = workbook.Sheets[sheetName];

    //        // Convert the sheet data to an array of objects
    //        const result = XLSX.utils.sheet_to_json(sheet, { header: 1 });

    //        const headers = result[0];

    //        // Map the data to JSON objects
    //        const jsonDataArray = result.slice(1).map(data => {
    //            const jsonObject = {};
    //            headers.forEach((header, index) => {
    //                jsonObject[header] = data[index];
    //            });
    //            return jsonObject;
    //        });

    //        // jsonDataArray now contains the data in JSON format
    //        console.log(JSON.stringify(jsonDataArray, null, 2));
    //    };

    //    reader.readAsBinaryString(selectedFile);
    //});


    /// Excel File Load Data

    function exportJsonToExcel(jsonData, fileName) {
        const ws = XLSX.utils.json_to_sheet(jsonData);
        const wb = XLSX.utils.book_new();
        XLSX.utils.book_append_sheet(wb, ws, "Sheet1");
        XLSX.writeFile(wb, fileName);
    }

    const jsonData = [
        {
            "ItemId": 1,
            "ItemName": "Barcode100",
            "Qty": 10,
            "PVDate": 45186
        },
        {
            "ItemId": 2,
            "ItemName": "Barcode 200",
            "Qty": 200,
            "PVDate": 45186
        },
        {
            "ItemId": 3,
            "ItemName": "Barcode 300",
            "Qty": 300,
            "PVDate": 45186
        }
    ];

    document.getElementById("exportButton").addEventListener("click", function () {
        exportJsonToExcel(jsonData, "exported_data.xlsx");
    });


</script>
```
# Large Json Data Provide

```
 <system.web.extensions>
        <scripting>
            <webServices>
                <jsonSerialization maxJsonLength="5000000" /> <!-- Set to your desired length -->
            </webServices>
        </scripting>
    </system.web.extensions>

 List<Person> data = new List<Person>();

            for (int i = 0; i < 5000000; i++)
            {
                var person = new Person
                {
                    Name = "John",
                    Age = 30
                };
                data.Add(person);
            }
            DataTable dt = new DataTable();
            dt.Columns.Add("Name", typeof(string));
            dt.Columns.Add("Age", typeof(int));

            // Populate the DataTable from the List
            foreach (var person in data)
            {
                dt.Rows.Add(person.Name, person.Age);
            }
            return LargeJson(dt);
             
            //var item = JsonConvert.SerializeObject(dt);
            //JsonResult result = Json(item);
            //result.MaxJsonLength = Int32.MaxValue;
            //result.JsonRequestBehavior = JsonRequestBehavior.AllowGet;
            //return result;

  ActionResult LargeJson(DataTable dt)
        {
            var item = JsonConvert.SerializeObject(dt);
            JsonResult result = Json(item);
            result.MaxJsonLength = Int32.MaxValue;
            result.JsonRequestBehavior = JsonRequestBehavior.AllowGet;
            return result;
        }

```

# Java Script Serilizer
```
            //... create object if required.
            var rkObj = new { Success = Success, Message = Message, TableData = TableData };
            JavaScriptSerializer rkSerializer = new JavaScriptSerializer() { MaxJsonLength = Int32.MaxValue };
            ContentResult rkResult = new ContentResult() { Content = rkSerializer.Serialize(rkObj), ContentType = "application/json" };
            return rkResult;
        }
```




