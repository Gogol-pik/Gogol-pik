from tkinter import *
from tkinter import messagebox as mb
from tkinter import ttk
import math, time, sys, random, string

class Application(Tk, Frame):
    def __init__(self, *args, **kwargs): # kwargs (keyword-args) - переменное количество именованных аргументов (например: a=9, b=8, x=7). Упаковывает в словарь
        Tk.__init__(self)        
        self.geometry("830x600")
        self.title("Обезличивание текста методом шифровки или замены личных данных на их ID")
        self.attributes("-alpha", 1.00)
        self.attributes("-topmost", True)
        self.resizable(height=False, width=False)
        self["bg"] = "#000000"
        self.f = ttk.Label(self, text="Как обезличить ваш текст?", font="Courier 15", foreground="#ffffff", background="#000000")
        self.f.pack(fill=X)
        self.frame_for_buttons_and_drop_down_lists = Frame(self, background="#000000")
        self.frame_for_buttons_and_drop_down_lists.pack(fill=X)
        self.depersionalize_or_encrypt = ttk.Combobox(self.frame_for_buttons_and_drop_down_lists, values=["Заменить на ID", "Зашифровать звёздочками"], state="readonly", font="Courier 18",
                                                      width="24")
        self.depersionalize_or_encrypt.current(0)
        self.depersionalize_or_encrypt.pack(side=LEFT)
        btn_1 = Button(self.frame_for_buttons_and_drop_down_lists, text="Обезличить", font="Courier 18", foreground="#ffffff", background="#006600", command=self.__main__)
        btn_1.pack(side=LEFT)
        self.exit_but = Button(self.frame_for_buttons_and_drop_down_lists, text="Выйти", font="Courier 18", foreground="#ffffff", background="#ff0000", command=self.app_exit)
        self.exit_but.pack(fill=BOTH)
        
                
        self.l = Label(self, text="Введите текст:", font="Courier 24", foreground="#ffffff", background="#000000")
        self.l.pack(side=LEFT)
        self.e = Entry(self, font="Courier 24", foreground="#ffffff", background="#000000", width="65")
        self.e.pack(side=RIGHT)
        self.label = Label(self, text="Введите путь или имя текстового документа\nи кодировку,\nкуда хотите записать результат\nчерез пробел:", font="Courier 18", foreground="#ffffff", background="#000000")
        self.label.place(relx=0.5, rely=0.75, anchor=CENTER)
        
        self.filename = Entry(self, font="Courier 15", foreground="#ffffff", background="#000000", width="35")
        self.filename.place(relx=0.5, rely=0.90, anchor=CENTER)                
        
        
        

    def app_exit(self):
        self.destroy()
        sys.exit()


    def __main__(self):
        if self.depersionalize_or_encrypt.current() == 0:
            try:
                self.__ID__()
            except Exception as e:
                mb.showerror(title=Exception, message=e)
            


        elif self.depersionalize_or_encrypt.current() == 1:
            try:
                self.encrypt()
            except Exception as e:
                mb.showerror(title=Exception, message=e)
            
    def __ID__(self):
        self.txt = self.e.get()
        self.words=self.txt.split()
        for i, j in enumerate(self.words): # зашифровка паспортных данных
            if (self.words[i].lower() == "паспорт") or (self.words[i].lower() == "passport"):               
                self.words[i+2] = "****"
                self.words[i+4] = "******" if self.words[i+3] != "№" else "№******"

        for n, w in enumerate(self.words): # замена женских ФИО на их ID
            if (((self.words[n][-2:] == "ва" and self.words[n][0].isupper() == True) or (self.words[n][-2:] == "на" and self.words[n][0].isupper() == True) \
                or (self.words[n][-3:] == "вой" and self.words[n][0].isupper() == True) \
               or (self.words[n][-3:] == "ной" and self.words[n][0].isupper() == True) or (self.words[n][-2:] == "ву" and w[0].isupper() == True) \
                or (self.words[n][-2:] == "ну" and self.words[n][0].isupper() == True))): 
                self.words[(self.words.index(self.words[n]))] = str(id(self.words[(self.words.index(self.words[n]))]))
                self.words[(self.words.index(self.words[n]))+1]=str(id(self.words[(self.words.index(self.words[n]))+1]))
                self.words[(self.words.index(self.words[n]))+2]=str(id(self.words[(self.words.index(self.words[n]))+2]))

        for s, t in enumerate(self.words): # замена мужских ФИО на их ID
            if (((self.words[s][-1] == "в") and (self.words[s][0].isupper() == True)) or ((self.words[s][-1] == "н") and (self.words[s][0].isupper() == True)) \
               or ((self.words[s][-2:] == "ва") and (self.words[s][0].isupper() == True)) or \
               ((self.words[s][-3:] == "вым") and (self.words[s][0].isupper() == True)) or ((self.words[s][-3:] == "ным") and (self.words[s][0].isupper() == True))):
                self.words[s] = str(id(self.words[s]))
                self.words[s+1] = str(id(self.words[s+1]))
                self.words[s+2] = str(id(self.words[s+2]))

            elif (((self.words[s][-1] == "о") and (self.words[s].isupper() == True)) or ((self.words[s][-1] == "ц") and (self.words[s].isupper() == True)) \
                 or ((self.words[s][-1] == "й") and (self.words[s].isupper() == True))):
                self.words[(self.words.index(self.words[s]))] = str(id(self.words[(self.words.index(self.words[s]))]))
                self.words[(self.words.index(self.words[s]))+1] = str(id(self.words[(self.words.index(self.words[s]))+1]))
                self.words[(self.words.index(self.words[s]))+2] = str(id(self.words[(self.words.index(self.words[s]))+2]))
                                
        for a, b in enumerate(self.words): # замена дат рождения на их ID
            if ((b.lower())[0:5] == "родил"):
                self.words[(self.words.index(b))+1] = str(id(self.words[(self.words.index(b))+1]))
            elif (self.words[a].lower() == "was") and (self.words[a+1].lower() == "born"):
                self.words[(self.words.index("was"))+2] = str(id(self.words[(self.words.index("was"))+2]))

        for x, y in enumerate(self.words): # замена дат выдачи паспортов на их ID
            if ((self.words[x].lower() == "выдан") or (self.words[x].lower() == "выдана")):
                self.words[(self.words.index(y))+1] = str(id(self.words[(self.words.index(y))+1]))

            elif ((self.words[x].lower() == "received") and (self.words[x+1] == "on" or "in")):
                self.words[x+2] = str(id(self.words[x+2]))

        for index, value in enumerate(self.words):
            if ("выдал" in self.words[index])

        with open(self.filename.get().split()[0], 'a', encoding=self.filename.get().split()[1]) as s:
            s.write(' '.join(self.words) + "\n")

        mb.showinfo(title="Результат", message=f"Обезличенный текст - \"{' '.join(self.words)}\".\nОн записан в текстовый документ \"{self.filename.get().split()[0]}\" в кодировке \"{self.filename.get().split()[1]}\"")
        

    def encrypt(self):
        self.txt = self.e.get()
        self.words=self.txt.split()
        
        
        for i, j in enumerate(self.words): # зашифровка паспортных данных
            if (self.words[i].lower() == "паспорт") or (self.words[i].lower() == "passport"):               
                self.words[i+2] = "****"
                if self.words[i+3] != "№":
                    self.words[i+4] = "******"
                else:
                    self.words[i+3] = "№" + ('*' * len(self.words[i+3][1:]))

        for n, w in enumerate(self.words): 
            if (((self.words[n][-2:] == "ва" and self.words[n][0].isupper() == True) or (self.words[n][-2:] == "на" and self.words[n][0].isupper() == True) \
                or (self.words[n][-3:] == "вой" and self.words[n][0].isupper() == True) \
               or (self.words[n][-3:] == "ной" and self.words[n][0].isupper() == True) or (self.words[n][-2:] == "ву" and w[0].isupper() == True) \
                or (self.words[n][-2:] == "ну" and self.words[n][0].isupper() == True))):
                self.words[n] = f"{self.words[n][0]}{'*' * len(self.words[n][(1-len(self.words[n])):-1])}{self.words[n][-1]}"
                self.words[n+1] = f"{self.words[n][0]}{'*' * len(self.words[n+1][(1-len(self.words[n+1])):-1])}{self.words[n+1][-1]}"
                self.words[n+2] = f"{self.words[n][0]}{'*' * len(self.words[n+2][(1-len(self.words[n+2])):-1])}{self.words[n+2][-1]}"

        for s, t in enumerate(self.words):
            if (((self.words[s][-1] == "в") and (self.words[s][0].isupper() == True)) or ((self.words[s][-1] == "н") and (self.words[s][0].isupper() == True)) \
               or ((self.words[s][-2:] == "ва") and (self.words[s][0].isupper() == True)) or \
               ((self.words[s][-3:] == "вым") and (self.words[s][0].isupper() == True)) or ((self.words[s][-3:] == "ным") and (self.words[s][0].isupper() == True))):
                self.words[(self.words.index(self.words[s]))] = self.words[(self.words.index(self.words[s]))][0] + ('*' * \
                                                                                                                    len(self.words[(
                                                                                                                        self.words.index(self.words[s]))][1-\
                                                                                                                                                          len(self.words[self.words.index(self.words[s])]):-1])) + \
                                                                                                                                                          self.words[(self.words.index(self.words[s]))][-1]
                self.words[(self.words.index(self.words[s]))+1] = self.words[(self.words.index(self.words[s]))+1][0] + ('*' * \
                                                                                                                    len(self.words[(
                                                                                                                        self.words.index(self.words[s]))+1][1-\
                                                                                                                                                          len(self.words[self.words.index(self.words[s])+1]):-1])) + \
                                                                                                                                                          self.words[(self.words.index(self.words[s]))+1][-1]
                self.words[(self.words.index(self.words[s]))+2] = self.words[(self.words.index(self.words[s]))+2][0] + ('*' * \
                                                                                                                    len(self.words[(
                                                                                                                        self.words.index(self.words[s]))+2][1-\
                                                                                                                                                          len(self.words[self.words.index(self.words[s])+2]):-1])) + \
                                                                                                                                                          self.words[(self.words.index(self.words[s]))+2][-1]

            elif (((self.words[s][-1] == "о") and (self.words[s][0].isupper() == True)) or ((self.words[s][-1] == "ц") and (self.words[s][0].isupper() == True)) \
                 or ((self.words[s][-1] == "й") and (self.words[s][0].isupper() == True))):
                self.words[s] = f"{self.words[s][0]}{'*' * len(self.words[s][(1-len(self.words[s])):-1])}"
                self.words[s+1] = f"{self.words[s+1][0]}{'*' * len(self.words[s+1][(1-len(self.words[s+1])):-1])}"
                self.words[s+2] = f"{self.words[s+2][0]}{'*' * len(self.words[s+2][(1-len(self.words[s+2])):-1])}"

        for a, b in enumerate(self.words): 
            if ((b.lower())[0:5] == "родил"):
                self.words[(self.words.index(b))+1] = "*" * len(self.words[(self.words.index(b))+1])
            elif (self.words[a].lower() == "was") and (self.words[a+1].lower() == "born"):
                self.words[(self.words.index("was"))+2] = "*" * len(self.words[(self.words.index("was"))+2])
                

        for x, y in enumerate(self.words): 
            if ((self.words[x].lower() == "выдан") or (self.words[x].lower() == "выдана")):
                self.words[(self.words.index(y))+1] = '*' * len(self.words[(self.words.index(y))+1])

            elif ((self.words[x].lower() == "received") and (self.words[x+1] == "on" or "in")):
                self.words[x+2] = '*' * len(self.words[x+2])

        with open(self.filename.get().split()[0], 'a', encoding=self.filename.get().split()[1]) as s:
            s.write(' '.join(self.words) + "\n")

        mb.showinfo(title="Результат", message=f"Обезличенный текст - {' '.join(self.words)}.\nОн записан в текстовый документ \"{self.filename.get().split()[0]}\" в кодировке \"{self.filename.get().split()[1]}\"")
        
            

if __name__ == "__main__":
    root = Application(1,2,3, b=9, length=6, par=76)
    root.mainloop()
        
