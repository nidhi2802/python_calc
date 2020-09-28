# GUI based Calculator using Python based on MVC Structure


Here a simple calculator is created using python which follows the MVC architectural pattern which separates the application in three logical components: the model, the view and the controller

## Step 1. Create test.py
This file is created to check if the PyQt5 GUI toolkit is installed and working properly in your machine or not.
### Code Sample

```python
import sys

from PyQt5.QtWidgets import QApplication
from PyQt5.QtWidgets import QLabel
from PyQt5.QtWidgets import QWidget
```
Execute the test.py using:

```bash
python test.py
```
If while executing the file you get the error of ModuleNotFound suggests that PyQt5 is not installed. Install PyQt5 using the following command:

```bash
pip install PyQt5
```
Once installed execute the file and a small window will appear with message 'This is Test'.

## Step 2. Create view.py
The view.py here creates an application window for the user with the specific layout, title and size. The file contains two main functions _createDisplayLED() and _createButtons(). The _createDisplayLED() is for the display we see in the calculator and _createButtons() for the different buttons in the calculator.

### Code Sample for _createDisplayLED() and _createButtons()  
```python
def _createDisplayLED(self):
        """Create the display."""
        
        # Create the display widget
        self.display = QLineEdit()
        # Set some display's properties
        self.display.setFixedHeight(35)
        self.display.setAlignment(Qt.AlignRight)
        self.display.setReadOnly(True)
        
        # Add the display to the general layout
        self.generalLayout.addWidget(self.display)

    def _createButtons(self):
        """Create the buttons."""
        self.buttons = {}
        buttonsLayout = QGridLayout()
        # Button text | position on the QGridLayout
        buttons = {'7': (0, 0),
                   '8': (0, 1),
                   '9': (0, 2),
                   '/': (0, 3),
                   'C': (0, 4),
                   '4': (1, 0),
                   '5': (1, 1),
                   '6': (1, 2),
                   '*': (1, 3),
                   '(': (1, 4),
                   '1': (2, 0),
                   '2': (2, 1),
                   '3': (2, 2),
                   '-': (2, 3),
                   ')': (2, 4),
                   '0': (3, 0),
                   '00': (3, 1),
                   '.': (3, 2),
                   '+': (3, 3),
                   '=': (3, 4),
                  }

        # Create the buttons and add them to the grid layout
        for btnText, pos in buttons.items():
            self.buttons[btnText] = QPushButton(btnText)
            self.buttons[btnText].setFixedSize(40, 40)
            buttonsLayout.addWidget(self.buttons[btnText], pos[0], pos[1])
        # Add buttonsLayout to the general layout
        self.generalLayout.addLayout(buttonsLayout)
````
## Step 3. Create main.py
The main.py file is created to test the view.py layout. In the main.py import the GUI class from the view.
```python
#Import statements of main.py
import sys

from PyQt5.QtWidgets import QApplication
from view import GUI
```