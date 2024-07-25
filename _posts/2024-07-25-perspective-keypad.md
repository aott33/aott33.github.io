# Perspective Keypad Instructions

[Link to video instructions](https://drive.google.com/file/d/1IrF9fL5koWxKHQgli-_z-RLbuA8fXXw6/view?usp=sharing)

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
