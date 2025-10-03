import tkinter as tk

class Calculator(tk.Tk):
    def __init__(self):
        super().__init__()
        self.title("Calculator")
        self.resizable(False, False)
        self.geometry("320x420")
        self.expression = ""
        self.create_widgets()

    def create_widgets(self):
        self.display = tk.Entry(self, font=("Helvetica", 24), bd=0, relief=tk.RIDGE, justify='right')
        self.display.insert(0, "0")
        self.display.pack(fill='x', ipady=10, padx=10, pady=10)

        btns = [
            ('AC', 'C'), ('⌫', 'back'), ('(', '('), (')', ')'),
            ('7','7'), ('8','8'), ('9','9'), ('/','/'),
            ('4','4'), ('5','5'), ('6','6'), ('*','*'),
            ('1','1'), ('2','2'), ('3','3'), ('-','-'),
            ('0','0'), ('.','.'), ('=','='), ('+','+'),
        ]

        frame = tk.Frame(self)
        frame.pack(expand=True, fill='both')

        r = 0
        c = 0
        for (text, val) in btns:
            btn = tk.Button(frame, text=text, font=("Helvetica", 18), command=lambda v=val: self.on_click(v))
            btn.grid(row=r, column=c, sticky='nsew', padx=4, pady=4)
            c += 1
            if c > 3:
                c = 0
                r += 1

        # configure grid weights
        for i in range(4):
            frame.columnconfigure(i, weight=1)
        for i in range(r+1):
            frame.rowconfigure(i, weight=1)

    def on_click(self, char):
        if char == 'C':
            self.expression = ""
            self.display.delete(0, tk.END)
            self.display.insert(0, "0")
            return
        if char == 'back':
            self.expression = self.expression[:-1]
            self.display.delete(0, tk.END)
            self.display.insert(0, self.expression or "0")
            return
        if char == '=':
            try:
                # basic safety check
                if not all(ch in "0123456789+-*/(). " for ch in self.expression):
                    raise ValueError("Invalid input")
                result = eval(self.expression)
                self.expression = str(result)
            except Exception:
                self.expression = ""
                self.display.delete(0, tk.END)
                self.display.insert(0, "Error")
            else:
                self.display.delete(0, tk.END)
                self.display.insert(0, self.expression)
            return
        # append digit/operator
        self.expression += str(char)
        self.display.delete(0, tk.END)
        self.display.insert(0, self.expression)

if __name__ == "__main__":
    app = Calculator()
    app.mainloop()
