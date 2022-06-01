# Form

## step 1:
* create a google form
## step 2:
* Create a spreadsheet with data which is to be filled in the form
## Step 3:
* In spreadsheet go to extensions and click appscript
## Step 4:
* Write code in appscript

# Automatic Form Filling
```Javascript
function autoEntry() {
  var formURL = "https://docs.google.com/forms/d/e/1FAIpQLScZu1ay0KUu0p_W2adhi4FUHExjdR2EY_cQGKLPme8UqQdMjQ/formResponse";

  var wrkBk = SpreadsheetApp.getActiveSpreadsheet();
  var wrkSht = wrkBk.getSheetByName("data");
  for (i = 2; ; i++) {
    var name = wrkSht.getRange("A" + i).getValue();
    var project = wrkSht.getRange("B" + i).getValue();
    var date = wrkSht.getRange("C" + i).getValue();
    var time = wrkSht.getRange("D" + i).getValue();
    if (!name || !project || !date || !time) {
      break;
    }
    var datamap = {
      "entry.599843215": name,
      "entry.1565046053": project,
      "entry.1492787863": date,
      "entry.335492418": time
    }
    var options = {
      "method": "post",
      "payload": datamap
    };
    UrlFetchApp.fetch(formURL, options)
  }
}
```

# Form to Google Sheet
```Javascript
function onFormSubmit(event) {
  let l = 0
  let m = 1;
  let n = 2;
  let o = 3
  record_array = []

  var form = FormApp.openById('1GbtZi8jUr5bEqQRl1fJRTq6Ek79UGRkja4co7FzWZBg'); 
  var formResponses = form.getResponses();
  var formCount = formResponses.length;

  for (var i = 0; i < formCount; i++) {
    var formResponse = formResponses[i]
    var itemResponses = formResponse.getItemResponses()
    for (var j = 0; j < itemResponses.length; j++) {
      var itemResponse = itemResponses[j];
      console.log("item respnse is", itemResponse)
      var title = itemResponse.getItem().getTitle();
      var answer = itemResponse.getResponse();
      Logger.log(title);
      Logger.log(answer);
      record_array.push(answer);
    }

    AddRecord(record_array[l], record_array[m], record_array[n], record_array[o]);
    l = l + 4;
    m = m + 4;
    n = n + 4;
    o = o + 4;

  }
}

function AddRecord(name, project, date, time) {
  var url = 'https://docs.google.com/spreadsheets/d/1LhM_MJXm8VJ_BwpS1k9qE2fB2_eRAdBhPBhUvwxICHQ/edit#gid=0';  
  var ss = SpreadsheetApp.openByUrl(url);
  var dataSheet = ss.getSheetByName("form");
  dataSheet.appendRow([name, project, date, time]);
}
```