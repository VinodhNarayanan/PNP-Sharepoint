# PNP-Sharepoint
SharePoint Functionalities using pnp-js

Upload Multiple File attachments into sharepoint list item

Include Jquery and PNP Js from CDN

<script src="https://code.jquery.com/jquery-3.3.1.min.js" type="text/javascript"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/sp-pnp-js/3.0.10/pnp.min.js" type="text/javascript"></script>

Declare Global Array Variable to hold the multiple file content

 var fileInfos = [];
 
 Fire the onchange event using jquery to convert the uploaded files to blob using javascript FileReader()
 
  $(document).ready(function() {
        $('#exampleFormControlFile1').on('change', function() {
            blob();
        }); 
    });
    
   
   Create the Function "blob"
 
    function blob() {
        var input = document.getElementById("exampleFormControlFile1");   //get the file input
        var fileCount = input.files.length;   
        for (var i = 0; i < fileCount; i++) {
            var fileName = input.files[i].name;
            console.log(fileName);
            var file = input.files[i];
            var reader = new FileReader();
            reader.onload = (function(file) {
                return function(e) {
                    console.log(file.name);
                    fileInfos.push({                           //Push the file content into array
                        "name": file.name,
                        "content": e.target.result
                    });
                    console.log(fileInfos);
                }
            })(file);

            reader.readAsArrayBuffer(file);
        }
        //End of for loop
    }

Write the Upload logic using Pnp-js

function uploadListAttachments() {
        var item = $pnp.sp.web.lists.getByTitle("demopoc2").items.getById(1);   // Get the item id 
        //Pass the array value
        item.attachmentFiles.addMultiple(fileInfos).then(v => {
            console.log(v);
        }).catch(function(err) {
            alert(err);
        });
    }
    
Note: Attachment is uploaded is only after successfull creation of list item

Sharing is caring!.....
