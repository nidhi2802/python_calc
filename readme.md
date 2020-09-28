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
If while executing the file you get the error of ```ModuleNotFound``` suggests that PyQt5 is not installed. Install PyQt5 using the following command:

```bash
pip install PyQt5
```
Once installed execute the file and a small window will appear with message ```'This is Test'```.

## Step 2. Create view.py
The view.py here creates an application window for the user with the specific layout, title and size. The file contains two main functions ```_createDisplayLED()``` and ```_createButtons()```. The ```_createDisplayLED()``` is for the display we see in the calculator and ```_createButtons()``` for the different buttons in the calculator.

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
The ```main.py``` file is created to test the ```view.py``` layout. In the ```main.py``` import the GUI class from the view.
```python
#Import statements of main.py
import sys

from PyQt5.QtWidgets import QApplication
from view import GUI
```
## Step 4. Create model.py
 This file has only one function ```evaluateExpression(expression)``` which will be used to evaluate the expression given by the user and return the result and if any error occurs the functions returns the error message.
```python
ERROR_MSG = 'ERROR'
# Create a Model to handle the calculator's operation
def evaluateExpression(expression):
    """Evaluate an expression."""
    try:
        result = str(eval(expression, {}, {})) 
    except Exception:
        result = ERROR_MSG

    return result
```

## Step 5. Update main.py
The ```evaluateExpression(expression)``` from ```model.py``` will be imported in the file using the import statement and will be called in the ```main.py``` to evaluate the input from the user.
### Added statements in main.py
```python
from  model import evaluateExpression

def main():
    model = evaluateExpression
```
## Step 6. Create controller.py
This file has function name ```_connectSignals(self)``` which is used to capture all the events by calling different functions like ```_calculateResult(self)``` and ```_buildExpression(self, sub_exp)```. ```_buildExpression(self, sub_exp)``` creates expression by appending the characters and display it in the display box.
### Code for  _connectSignals(), _calculateResult() and _buildExpression()
```python
def _calculateResult(self):
        """Evaluate expressions."""
        result = self._evaluate(expression=self._view.getDisplayText())
        self._view.setDisplayText(result)

    def _buildExpression(self, sub_exp):
        """Build expression."""
        if self._view.getDisplayText() == ERROR_MSG:
            self._view.clearDisplay()

        expression = self._view.getDisplayText() + sub_exp
        self._view.setDisplayText(expression)

    def _connectSignals(self):
        """Connect signals and slots."""
        for btnText, btn in self._view.buttons.items():
            if btnText not in {'=', 'C'}:
                btn.clicked.connect(partial(self._buildExpression, btnText))

        self._view.buttons['='].clicked.connect(self._calculateResult)
        self._view.display.returnPressed.connect(self._calculateResult)
        self._view.buttons['C'].clicked.connect(self._view.clearDisplay)
```


## Step 7. Update main.py
In the ```main.py``` file now import the Controller class from ```controller.py``` using the import statements and add import statements for controller before the import statements for model. After this connect the Controller with view by passing ```model``` and ```view``` to the ```Controller```.
### Statements added in main.py
```python
from controller import Controller

def main():
    Controller(model=model, view=view)
```