---
title: "Perspective Keypad Instructions"
date: 2024-07-25 00:00:00 -0400
categories: [SCADA, Ignition]
tags: [ignition, perspective, keypad, hmi, scada]
description: "Step-by-step guide to adding and configuring number keypads in Ignition Perspective for HMI applications"
toc: true
---

<video width="100%" src="https://github.com/user-attachments/assets/38f469f8-0fd3-49f7-b0ba-75ebc349f242" controls="controls">
</video>

## Adding Number Keypad to Ignition Project
1. Download the keypad from [here](https://inductiveautomation.com/exchange/2380/installation)
2. Unzip download to access correct zip file
	<table><tr><td>
		<img src="https://github.com/user-attachments/assets/0e72774f-1098-4ef4-b734-519f16cb802c"/>
	</td></tr></table>
3. In Designer, select File > Import
	<table><tr><td>
		<img src="https://github.com/user-attachments/assets/c563183b-6bfd-455c-9679-fa880c16bd10"/>
	</td></tr></table>
4. In Open pop up, select the keypad.zip file and click open
	 <table><tr><td>
		<img src="https://github.com/user-attachments/assets/9e590864-5606-4000-9d78-615070a5114f"/>
	</td></tr></table>
5. In Import pop up, Select all items and select Import
	<table><tr><td>
		<img src="https://github.com/user-attachments/assets/0c77542e-a000-439f-bf84-8bbade398a04"/>
	</td></tr></table>

## Creating popup
1. Navigate to the view with the numeric entry field
2. Deep select the numeric entry field
3. Setup custom properties
  - Select “Add Custom Property”
  - Select “Object” and type “keypad” all lowercase
		<table><tr><td>
			<img src="https://github.com/user-attachments/assets/671443c0-ce60-4977-bf97-e6d5b7fd161e"/>
		</td></tr></table>
  - Select “Add Object Members” and add the following items: title, units, centerScreen
		<table><tr><td>
			<img src="https://github.com/user-attachments/assets/b5b14a87-87e0-4ee6-bbc6-4f8f7d0cacf9"/>
		</td></tr></table>
4. Setup the popup
  - Right click the numeric entry field and select “Configure Event” or type “ctrl + j”
		<table><tr><td>
			<img src="https://github.com/user-attachments/assets/b092b445-6964-4f64-a7c3-453c3220a746"/>
		</td></tr></table>
  - Select Mouse Events > onMouseDown then click the plus icon and select “Script”
		<table><tr><td>
			<img src="https://github.com/user-attachments/assets/414bff67-f0db-4a0a-8688-569abf3483ed"/>
		</td></tr></table>
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
