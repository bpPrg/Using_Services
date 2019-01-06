# Using python module gspread (google spreadsheets)
https://gspread.readthedocs.io/en/latest/oauth2.html   
https://github.com/burnash/gspread    
https://developers.google.com/identity/protocols/OAuth2  
```python
# install the module in dataSc environment
source activate dataSc
which pip
/Users/poudel/miniconda3/envs/dataSc/bin/pip install gspread
/Users/poudel/miniconda3/envs/dataSc/bin/pip install --upgrade google-api-python-client oauth2client
/Users/poudel/miniconda3/envs/dataSc/bin/pip install PyOpenSSL
```
- go to https://console.developers.google.com/
- go to Credentials > Create credentials > Service account key > select account > new account > 
 +  (NOTE: Now I have service account gspread, and can easily download the json from here)
- download the json to your local computer `~/bin/json_name.json`
- Now create a google spreadhseet in your google drive if you don't have already. 
  + `(e.g. ~/Google Drive/keep_this.gsheet)`
-  copy the client email that looks like `gspreadservice@gspread-xxxxxx.iam.gserviceaccount.com` from the json.
- share the spreadhsheet in google drive to this email.
- Now we can use the python module `gspread`

```python
import gspread
from oauth2client.service_account import ServiceAccountCredentials
import os
import glob

json_dir = os.path.expanduser('~') + '/bin/'
gspread_json = glob.glob(json_dir + 'gspread*.json')[0]
scope = ['https://spreadsheets.google.com/feeds',
         'https://www.googleapis.com/auth/drive']
credentials = ServiceAccountCredentials.from_json_keyfile_name(gspread_json, scope)
gc = gspread.authorize(credentials)

# I have a gsheet called keep_this.gsheet in my google drive
wks = gc.open("keep_this").sheet1
wks = gc.open_by_key('0BmgG6nO_6dprxxx')
wks = gc.open_by_url('https://docs.google.com/spreadsheet/ccc?key=xxxx')

# attributes
print([ i for i in dir(wks) if not i.startswith('_')])

# manipulation
wks.cell(1, 2).value # index starts from 1 NOT FROM ZERO
wks.row_values(1)
wks.col_values(1)
wks.get_all_values()
wks.find("author")  # <Cell R2C1 'author'>
```
