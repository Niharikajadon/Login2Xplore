!DOCTYPE html>
<!--
To change this license header, choose License Headers in Project Properties.
To change this template file, choose Tools | Templates
and open the template in the editor.
-->
<html lang="en">
    <head>
        <title>Bootstrap Example</title>
        <meta charset="utf-8">
        <meta name="viewport" content="width=device-width, initial-scale=1">
        <link rel="stylesheet"
              href="https://maxcdn.bootstrapcdn.com/bootstrap/3.4.1/css/bootstrap.min.css">
        <script
        src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
        <script
        src="https://maxcdn.bootstrapcdn.com/bootstrap/3.4.1/js/bootstrap.min.js"></script>
    </head>
    <body>
        <div class="container">
            <h2>Vertical (basic) form</h2>
            <form id="empForm" method="post">
                <div class="form-group">
                    <span><label for="empId">Employee ID:</label> <label id="empIdMsg">
                        </label></span>
                    <input type="text" class="form-control" name="empId" id="empId"
                           placeholder="Enter Employee ID" required>
                </div>
                <div class="form-group">
                    <label for="empName">Employee Name:</label>
                    <input type="text" class="form-control" id="name"
                           placeholder="Enter Employee Name" name="name">
                </div>
                <div class="form-group">
                    <label for="empEmail">Email:</label>
                    <input type="email" class="form-control" id="email"
                           placeholder="Enter Employee Email" name="email">
                </div>
                <input type="button" class="btn btn-primary" id="empSave" value="Save"
                       onclick="saveEmployee();">
            </form>
        </div>
        <script>
            $("#empId").focus();
            function validateAndGetFormData() {
                var empIdVar = $("#empId").val();
                if (empIdVar === "") {
                    alert("Employee ID Required Value");
                    $("#empId").focus();
                    return "";
                }
                var nameVar = $("#name").val();
                if (nameVar === "") {
                    alert("Employee Name is Required Value");
                    $("#name").focus();
                    return "";
                }
                var emailVar = $("#email").val();
                if (emailVar === "") {
                    alert("Employee Email is Required Value");
                    $("#email").focus();
                    return "";
                }
                var jsonStrObj = {
                    empId: empIdVar,
                    name: nameVar,
                    email: emailVar,
                };
                return JSON.stringify(jsonStrObj);
            }
            // This method is used to create PUT Json request.
            function createPUTRequest(connToken, jsonObj, dbName, relName) {
                var putRequest = "{\n"
                        + "\"token\" : \""
                        + connToken
                        + "\","
                        + "\"dbName\": \""
                        + dbName
                        + "\",\n" + "\"cmd\" : \"PUT\",\n"
                        + "\"rel\" : \""
                        + relName + "\","
                        + "\"jsonStr\": \n"
                        + jsonObj
                        + "\n"
                        + "}";
                return putRequest;
            }
            function executeCommand(reqString, dbBaseUrl, apiEndPointUrl) {
                var url = dbBaseUrl + apiEndPointUrl;
                var jsonObj;
                $.post(url, reqString, function (result) {
                    jsonObj = JSON.parse(result);
                }).fail(function (result) {
                    var dataJsonObj = result.responseText;
                    jsonObj = JSON.parse(dataJsonObj);
                });
                return jsonObj;
            }
            function resetForm() {
                $("#empId").val("")
                $("#name").val("");
                $("#email").val("");
                $("#empId").focus();
            }
            function saveEmployee() {
                var jsonStr = validateAndGetFormData();
                if (jsonStr === "") {
                    return;
                }
                var putReqStr = createPUTRequest("90935601|-31948840339110952|90933284",
                        jsonStr, "Employee", "Emp-Rel");
                alert(putReqStr);
                jQuery.ajaxSetup({async: false});
                var resultObj = executeCommand(putReqStr, "http://api.login2explore.com:5577", "/api/iml");
                alert(JSON.stringify(resultObj));
                jQuery.ajaxSetup({async: true});
                resetForm();
            }
        </script>
    </body>
</html>
