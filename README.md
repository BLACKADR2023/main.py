# main.py
from flet import *
import sqlite3
from pywebio import input 
 

def delete_student_ui():  
    student_id = input.input("Enter Student ID to delete:")  
    delete_student_ui(student_id)

conn =sqlite3.connect('dato.db',check_same_thread=False)
cursor=conn.cursor()
cursor.execute("""CREATE TABLE IF NOT EXISTS student(
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    stname TEXT,       
    stmail TEXT,
    stphone TEXT,
    stadresse TEXT,       
    smath INTEGER,
    sarab INTEGER,
    sfrancais INTEGER,
    senglish INTEGER,
    sdraw INTEGER,
    schemistry INTEGER
)""")
conn.commit()


def main(page:Page):
    page.title='kharroubi mohammed'
    page.scroll='auto'
    page.window.top= 1
    page.window.left= 960
    page.window.width= 390
    page.window.height=740
    page.bgcolor='white'
    page.theme_mode =ThemeMode.LIGHT
    ############################################
    table_name='student'
    query=f'SELECT COUNT(*) FROM {table_name}'
    cursor.execute(query)
    result =cursor.fetchone()
    row_count =result [0]


    def add(e):  
        try:  
            cursor.execute("INSERT INTO student (stname,stmail,stphone,stadresse,smath,sarab,sfrancais,senglish,sdraw,schemistry) VALUES (?,?,?,?,?,?,?,?,?,?)",  
                       (tname.value, tmail.value, tphone.value, tadresse.value, math.value, arab.value, francais.value, english.value, draw.value, chemistry.value))  
            conn.commit()  
            page.add(Text("Student added successfully!", color='green'))  
        except Exception as ex:  
            page.add(Text(f"Error: {str(ex)}", color='red'))
    def show(e):
        c=conn.cursor()
        c.execute("SELECT * FROM student")
        users=c.fetchall()
        print(users)
    

    def delete_student(student_id):  
    
        try:  
        # تأكد من أن هناك إدخال بالمعرف الذي تريد حذفه  
            cursor.execute("DELETE FROM student WHERE id = ?", (student_id,))  
        
        # تحقق من عدد الصفوف التي تم حذفها  
            if cursor.rowcount > 0:  
                conn.commit()  # قم بتحديث قاعدة البيانات  
                print(f"Student with ID {student_id} has been deleted successfully.")  
            else:  
                print(f"No student found with ID {student_id}.")  

        except Exception as ex:  
            print(f"Error: {str(ex)}")
    
    

    
    ############# feilds ###########
    tname=TextField(label='first&last Name',icon=icons.PERSON,height=38)
    tmail=TextField(label='Email',icon=icons.MAIL,height=38)
    tphone=TextField(label='Phone',icon=icons.PHONE,height=38)
    tadresse=TextField(label='Adresse',icon=icons.LOCATION_CITY,height=38)
    #############marks ###############
    marktext=Text("Marks of test :",text_align='center',font_family='cooper',size=20)
    ##################################
    math=TextField(label='math',width=110,rtl=True,height=38)
    arab=TextField(label='Arab',width=110,rtl=True,height=38)
    francais=TextField(label='Francais',width=110,rtl=True,height=38)
    english=TextField(label='English',width=110,rtl=True,height=38)
    draw=TextField(label='Draw',width=110,rtl=True,height=38)
    chemistry=TextField(label='Chemistry',width=110,rtl=True,height=38)
    ########################################
    addbutton=ElevatedButton("Add new student",width=170,style=ButtonStyle(bgcolor='blue',
                             color='black',padding=15    ),
                             on_click=add
        )
    showbutton=ElevatedButton("Show All Students",width=170,style=ButtonStyle(bgcolor='blue',
                              color='black',padding=15),
                             on_click=show                                                
        )
    delettebutton=ElevatedButton("deltte Student",width=170,style=ButtonStyle(bgcolor='blue',
                              color='black',padding=15),
                             on_click=delete_student
        )                     
    page.add(
        Row([
            Image(src='stud.gif',width=260)
        ],alignment='center'),
        Row([
            Text("[Student Management]",size=20,font_family='cooper',color='black')
        ],alignment='center'),
        Row([
            Text("list of registered students :",size=20,font_family='cooper',color='blue'),
            Text(row_count,size=20,font_family='cooper',color='red')
            ]),
            tname,tmail,tphone,tadresse,marktext,
        Row([
            math,arab,francais
        ]),
        Row([
            english,draw,chemistry
        ]),
        Row([
            addbutton,showbutton
        ],alignment='center'),
        Row([
            delettebutton
        ],alignment='center'),
    )
    page.update()
app(main)
