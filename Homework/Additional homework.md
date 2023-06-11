# Installing newman
## 1. install node.js
## in Command Line check:
```
node -v
npm -v
npm install -g newman
mewman run collection.json -e environment.json
```
to make a report and export it from newman to your PC:
JSON reporter:
```
newman run collection.json -e environment.json -r json --reporter-json-export file_name.json
```
Newman will save report in PC/Users/Admin with the written name.
or
XML reporter:
```
newman run collection.json -e environment.json -r junit --reporter-junit-export file_name-in junit.xml
```
or
HTML reporter
```
npm install -g  newman-reporter-html
newman run collection.json -e environment.json --reporters html 
```
or
```
newman run collection.json -e environment.json -r html 
```
Newman will save report in PC/Users/Admin
to create more advansed report in html:
download  from terminal `htmlextra`:
```
npm install -g newman-reporter-htmlextra 
```
then create report:
```
newman run collection.json -e environment.json -r htmlextra
```
