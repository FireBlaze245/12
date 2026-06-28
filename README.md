``` python
import sys
import pymysql
from PyQt6.QtWidgets import QMainWindow, QTableWidgetItem, QApplication
from PyQt6.uic import loadUi

class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()
        self.ui=loadUi("main.ui", self)
        conn = pymysql.connect(port=3306, host='localhost', user='root', password='root', database='var1')
        cursor = conn.cursor()
        cursor.execute("SELECT * FROM var1.new_view;")
        rows = cursor.fetchall()
        headers=[desc[0] for desc in cursor.description]
        self.ui.tableWidget.setRowCount(len(rows))
        self.ui.tableWidget.setColumnCount(len(headers))
        self.ui.tableWidget.setHorizontalHeaderLabels(headers)
        for idx, row in enumerate(rows):
            for j in range (len(headers)):
                self.ui.tableWidget.setItem(idx, j, QTableWidgetItem(str(row[j])))
if __name__ == "__main__":
    app = QApplication(sys.argv)
    window = MainWindow()
    window.show()
    sys.exit(app.exec())
```


