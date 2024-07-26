# Perspective Keypad Instructions

<video src="https://private-user-images.githubusercontent.com/48938478/352308653-38f469f8-0fd3-49f7-b0ba-75ebc349f242.mp4?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MjE5NDgwOTgsIm5iZiI6MTcyMTk0Nzc5OCwicGF0aCI6Ii80ODkzODQ3OC8zNTIzMDg2NTMtMzhmNDY5ZjgtMGZkMy00OWY3LWIwYmEtNzVlYmMzNDlmMjQyLm1wND9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDA3MjUlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQwNzI1VDIyNDk1OFomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTc5YTkyOTE4NWM1NGQ1Zjk4OTY3ZGJlYjAwNmQ2MmFkNDdkMjNhM2ViYzU5NDNlOTZmNjkxNzNlNTUxYmE1ZTMmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.kijvTZInAtTomFOIhaMCLxLRsoYHRCTtJiDF3RNofWE" data-canonical-src="https://private-user-images.githubusercontent.com/48938478/352308653-38f469f8-0fd3-49f7-b0ba-75ebc349f242.mp4?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MjE5NDgwOTgsIm5iZiI6MTcyMTk0Nzc5OCwicGF0aCI6Ii80ODkzODQ3OC8zNTIzMDg2NTMtMzhmNDY5ZjgtMGZkMy00OWY3LWIwYmEtNzVlYmMzNDlmMjQyLm1wND9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDA3MjUlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQwNzI1VDIyNDk1OFomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTc5YTkyOTE4NWM1NGQ1Zjk4OTY3ZGJlYjAwNmQ2MmFkNDdkMjNhM2ViYzU5NDNlOTZmNjkxNzNlNTUxYmE1ZTMmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.kijvTZInAtTomFOIhaMCLxLRsoYHRCTtJiDF3RNofWE" controls="controls" style="max-height:640px; min-height: 200px">
</video>

## Adding Number Keypad to Ignition Project
1. Download the keypad from here
2. Unzip download to access correct zip file

3. In Designer, select File > Import

4. In Open pop up, select the keypad.zip file and click open

5. In Import pop up, Select all items and select Import

## Creating popup
1. Navigate to the view with the numeric entry field
2. Deep select the numeric entry field
3. Setup custom properties
  - Select “Add Custom Property”
  - Select “Object” and type “keypad” all lowercase
  - Select “Add Object Members” and add the following items:
		title, units, centerScreen
		
7. Setup the popup
  - Right click the numeric entry field and select “Configure Event” or type “ctrl + j”
  - Select Mouse Events > onMouseDown then click the plus icon and select “Script”
  - Paste the following script into Script field and select “OK”
    ```
      popParams = {'owner': self.meta.name, 'min': self.props.inputBounds.minimum, 'max': self.props.inputBounds.maximum, 'oldValue': self.props.value, 'units': self.custom.keypad.units}
      posit = None if self.custom.keypad.centerScreen else {'left': event.clientX, 'top': event.clientY}
      system.perspective.openPopup('keypadPopup-' + self.meta.name, 'Exchange/Keypad/Numeric Popup', title=self.custom.keypad.title, params=popParams, position=posit, showCloseIcon=True, draggable=True, resizable=True, viewportBound=True, modal=True, overlayDismiss=True)
    ```
  - Right click the numeric entry field and select “Configure Scripts” or type “ctrl + k”
  - Select “+ Add Handler”
  - Type “keypadReturn” in the Message Type field
  - Paste the following script in the Script field:
    ```
      if self.meta.name == payload['owner']:
  		  self.props.value = float(payload['value'])
    ```
5. Click save

## Notes
* Each input field should have a unique name as this is passed as the "owner" of the popup so that only that input field receives the value that is entered on the popup.
