- 👋 Hi, I’m @phbat1012
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...
import sys
import random
import pandas as pd
from PyQt5.QtWidgets import QApplication, QWidget, QVBoxLayout, QPushButton, QLabel, QDesktopWidget, QFileDialog

class RandomRollCallApp(QWidget):
    def __init__(self):
        super().__init__()
        self.initUI()

    def initUI(self):
        self.setWindowTitle('随机点名系统')

        self.label_result = QLabel(self)
        self.label_result.setText("点击按钮选择模板文件")
        self.label_result.setAlignment(Qt.AlignmentFlag.AlignCenter)

        self.btn_select_template = QPushButton('选择模板文件', self)
        self.btn_select_template.clicked.connect(self.select_template)

        self.label_template_info = QLabel(self)
        self.label_template_info.setText("")

        self.btn_select_file = QPushButton('选择名单文件', self)
        self.btn_select_file.clicked.connect(self.select_file)
        self.btn_select_file.setEnabled(False)

        self.btn_roll_call = QPushButton('点名', self)
        self.btn_roll_call.clicked.connect(self.roll_call)
        self.btn_roll_call.setEnabled(False)

        vbox = QVBoxLayout()
        vbox.addWidget(self.label_result)
        vbox.addWidget(self.btn_select_template)
        vbox.addWidget(self.label_template_info)
        vbox.addWidget(self.btn_select_file)
        vbox.addWidget(self.btn_roll_call)
        self.setLayout(vbox)

        self.resize_to_screen()

    def resize_to_screen(self):
        # 获取屏幕的尺寸
        screen = QDesktopWidget().screenGeometry()
        self.resize(screen.width() // 3, screen.height() // 3)

    def select_template(self):
        options = QFileDialog.Options()
        options |= QFileDialog.DontUseNativeDialog
        filename, _ = QFileDialog.getOpenFileName(self,"选择模板文件", "","Excel Files (*.xlsx);;All Files (*)", options=options)
        if filename:
            self.template_file = filename
            self.label_template_info.setText(f"已选择模板文件: {filename}")
            self.btn_select_file.setEnabled(True)

    def select_file(self):
        options = QFileDialog.Options()
        options |= QFileDialog.DontUseNativeDialog
        filename, _ = QFileDialog.getOpenFileName(self,"选择名单文件", "","Excel Files (*.xlsx);;All Files (*)", options=options)
        if filename:
            self.selected_file = filename
            self.label_result.setText(f"已选择名单文件: {filename}")
            self.btn_roll_call.setEnabled(True)

    def roll_call(self):
        try:
            # 读取模板文件
            df_template = pd.read_excel(self.template_file)
            # 读取名单文件
            df = pd.read_excel(self.selected_file)
            # 将名单数据写入模板文件
            df_template['姓名'] = df['姓名']
            # 保存新文件
            output_file = self.selected_file.replace('.xlsx', '_output.xlsx')
            df_template.to_excel(output_file, index=False)
            self.label_result.setText(f"名单已写入新文件: {output_file}")
        except Exception as e:
            self.label_result.setText("出错，请检查文件路径或格式")

if __name__ == '__main__':
    app = QApplication(sys.argv)
    window = RandomRollCallApp()
    window.show()
    sys.exit(app.exec_())

<!---
phbat1012/phbat1012 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
