# Minimum-spanning-tree-MST
import copy
import time
import tkinter as tk
import random as rd
import math 
import ast

GUI = tk.Tk()
GUI.attributes('-fullscreen', True)

f_name = 0
k = 0
next = 0
ch = 0
np = 0
o = 0
file = 0
k = True
canvas = None
oval_cords = []
dist_list = []
min_dist_order_list = []
visited = []
final_tree = []

no_of_points_box = 0
no_of_points_box_entry = 0
time_b_val = 0
mode_b_val = 0
branch_b_val = 0

no_pt = 0

timee = 0
mode = 0 
branch = 0
total_cost = 0
f_name = 0
text = 0

def on_right_click(event):
    GUI.quit()
    
GUI.bind("<Button-3>", on_right_click)


def create_canvas():
    global canvas
    canvas = tk.Canvas(GUI, bg='#E0FFFF', height=GUI.winfo_height()-50, width=GUI.winfo_width())
    canvas.grid(row=0, column=0, sticky='nsew')
    return canvas


def random_cords():
    global canvas, np
    canvas = create_canvas()
    canvas.update()
    canvas_width = canvas.winfo_width()-10
    canvas_height = canvas.winfo_height()-100
    for i in range(np):
        x = rd.randint(0, canvas_width-15)
        y = rd.randint(0, canvas_height-100)
        canvas.create_oval(x - 5, y - 5, x + 5, y + 5, fill='blue')
        global oval_cords
        oval_cords.append([str(i),[x,y]])

        canvas.create_text(x + 10, y - 10, text=str(i), fill="red", font=("Arial", 10))

    dist_lst_calc()


def get_np():
    global np, no_of_points_box, no_of_points_box_entry

    random_btn.destroy()
    input_file_btn.destroy()
    choice_label.destroy()

    no_of_points_box = tk.Button(text='Enter the number of points in your minimum spanning tree here and press button to enter value: ',
                                command=np_calc , relief=tk.RAISED, bg = 'black', fg = 'white', font = ('Courier New',13))
    no_of_points_box.place(anchor = 'se', relx = 0.8, rely = 0.5)

    no_of_points_box_entry = tk.Entry(relief=tk.GROOVE, bg='grey')
    no_of_points_box_entry.place(anchor='se',relx = 0.5, rely = 0.56, width = 100, height = 25)

def np_calc():
    global np, no_of_points_box, no_of_points_box_entry

    np = no_of_points_box_entry.get()
    np = int(np)
    no_of_points_box.destroy()
    no_of_points_box_entry.destroy()    
    
    method()


def dist_lst_calc():
    global oval_cords, dist_list, min_dist_order_list, np
    global timee, mode, branch

    for j in range(np):
        x1 = oval_cords[j][1][0]
        y1 = oval_cords[j][1][1]


        for k in range(j+1, np):
            x2 = oval_cords[k][1][0]
            y2 = oval_cords[k][1][1]
            dist = round(math.sqrt((x2 - x1)**2 + (y2 - y1)**2), 5)
            dist_list.append([dist, [j, k]])
    min_dist_order_list = sorted(dist_list, key=lambda x: x[0])

    graph_lines_calc()


def root(lst, index):
    if lst[index][1] == -1:
        return index
    else:
        lst[index][1] = root(lst, lst[index][1])
        return lst[index][1]
    

def method():
    global time_b_val, mode_b_val, branch_b_val

    w_msg.destroy()

    if o == 1:
        f_name.destroy()
        k.destroy()

    instructions  = tk.Label(text = '''                             **INSTRUCTIONS**:
                               ------------

1. To run code in manual mode give mode as 1 and to run code in automatic mode give mode as 0

2. Input the time between every code run in the *TIME* box. (For instant output and manual mode give t as 0)

3. Enter the numbers of braches to connect at a time in the *BRANCH* box. (Branch >0)

                    Press *EXECUTE* button after parameter are given''',  font = ('Courier New',13), fg = 'white', bg = 'black',
    anchor = 'w', justify= 'left')
    instructions.place(relx = 0, rely = 0)
    
    time_b = tk.Label(text= 'Time', font = ('Courier New',11) , fg = 'white', bg = 'black')
    time_b.place(relx = 0, rely = 0.28)

    time_b_val = tk.Entry(relief=tk.GROOVE, bg='grey')
    time_b_val.place(relx = 0.05, rely = 0.28)


    mode_b = tk.Label(text='Mode', font = ('Courier New',11) , fg = 'white', bg = 'black')
    mode_b.place(relx = 0, rely = 0.33)

    mode_b_val = tk.Entry(relief=tk.GROOVE, bg='grey')
    mode_b_val.place(relx = 0.05, rely = 0.33)


    brach_b = tk.Label(text = 'Branch',font = ('Courier New',11) , fg = 'white', bg = 'black')
    brach_b.place(relx = 0, rely = 0.38)

    branch_b_val = tk.Entry(relief=tk.GROOVE, bg='grey')
    branch_b_val.place(relx = 0.05, rely = 0.38)

    enter_b = tk.Button(text = 'Execute', command = lambda: method_sec(), font = ('Courier New',11) , fg = 'white', bg = 'red',
    activeforeground = 'blue')
    enter_b.place(relx = 0.21, rely = 0.33)


def method_sec():
    global timee,mode,branch, canvas
    global canvas, np, min_dist_order_list

    timee = int(time_b_val.get())
    mode = int(mode_b_val.get())
    branch = int(branch_b_val.get())

    if o == 0:
        random_cords()
    elif o == 1:
        cords()


def sui():
    global k, no_pt, np, next, ch, n
    global visited, min_dist_order_list, oval_cords, canvas, final_tree , total_cost

    n = 0

    pseudo_lst = copy.deepcopy(visited)
    while pseudo_lst != []:
        c = pseudo_lst.pop()
        if c[1] == -1:
            n+=1

    if mode == 0:

        if len(min_dist_order_list) > 0:
            pts = min_dist_order_list.pop(0)

            from_pt = pts[1][0]
            to_pt = pts[1][1]

            parent_1 = root(visited, from_pt)
            parent_2 = root(visited, to_pt)

            if parent_1 != parent_2:

                if timee != 0:
                            GUI.update()
                            time.sleep(timee)

                visited[parent_2][1] = parent_1
                final_tree.append(pts)
                total_cost += pts[0]

                for p in oval_cords:
                    if int(p[0]) == from_pt:
                        x1 = p[1][0]
                        y1 = p[1][1]
                    elif int(p[0]) == to_pt:
                        x2 = p[1][0]
                        y2 = p[1][1]
                        canvas.create_line(x1,y1,x2,y2,fill = 'blue')
                        no_pt += 1
                        
            else:
                sui()


        elif n == 1:
            msg = tk.Label(text = 'MST constructed :)',  font = ('Courier New',11), fg = 'black', bg = 'light blue')
            msg.place(relx = 0.46, rely = 0.96)

            ch = tk.Button(text = 'Press button to store data in file', font = ('Courier New',11), fg = 'black', 
            bg = 'light blue', command = data_to_file, relief = 'raised')
            ch.place(relx = 0.05, rely = 0.96)

        else:
            msg = tk.Label(text = 'MST constructed :)',  font = ('Courier New',11), fg = 'black', bg = 'light blue')
            msg.place(relx = 0.46, rely = 0.96)

            ch = tk.Button(text = 'Press button to store data in file', font = ('Courier New',11), fg = 'black', 
            bg = 'light blue', command = data_to_file, relief = 'raised')
            ch.place(relx = 0.05, rely = 0.96)
            

    elif mode == 1:

        for t in range(branch):
            if len(min_dist_order_list) > 0:
                pts = min_dist_order_list.pop(0)

                from_pt = pts[1][0]
                to_pt = pts[1][1]

                parent_1 = root(visited, from_pt)
                parent_2 = root(visited, to_pt)


                if parent_1 != parent_2:

                    if timee != 0:
                                GUI.update()
                                time.sleep(timee)

                    visited[parent_2][1] = parent_1
                    final_tree.append(pts)
                    total_cost += pts[0]
                    
                    for p in oval_cords:
                        if int(p[0]) == from_pt:
                            x1 = p[1][0]
                            y1 = p[1][1]
                        elif int(p[0]) == to_pt:
                            x2 = p[1][0]
                            y2 = p[1][1]
                            canvas.create_line(x1,y1,x2,y2,fill = 'blue')
                            no_pt += 1


                else:
                    sui()

            else:
                next.destroy()
                msg = tk.Label(text = 'MST constructed :)',  font = ('Courier New',11), fg = 'black', bg = 'light blue')
                msg.place(relx = 0.46, rely = 0.96)

                ch = tk.Button(text = 'Press button to store data in file', font = ('Courier New',11), fg = 'black', 
                bg = 'light blue', command = data_to_file )
                ch.place(relx = 0.05, rely = 0.96)

                break


def data_to_file():
    global file, text
    ch.destroy()

    text = tk.Button(text = 'Enter file path and press button: ',  font = ('Courier New',11), fg = 'white', 
                bg = 'black',command = enter_data)
    text.place(relx = 0, rely = 0.96)

    file = tk.Entry(relief=tk.GROOVE, bg='grey')
    file.place(relx = 0.25, rely = 0.96)


def enter_data():
    global file, total_cost
    f = file.get()

    with open(f,'w+') as fi:

        while len(final_tree) > 0:
            fo = final_tree.pop(0)
            fi.write(str(fo) + '\n')

        fi.write('\n')
        fi.write('Total cost' + str(total_cost))

    file.destroy()
    text.destroy()


def graph_lines_calc():
    global k, n, no_pt
    global min_dist_order_list, oval_cords, canvas, visited, next
    global timee, mode, branch

    if len(visited) <np:
        for i in range(np):
            visited.append([i,-1])


    if mode == 0:

            while no_pt < (np - 1) and len(min_dist_order_list) > 0:
                for t in range(branch):
                    sui()

    elif mode == 1:

        next = tk.Button(text = 'NEXT', command = sui, fg = 'black', bg = 'red')
        next.place(relx=0.5, rely=0.98, anchor='se')  


def get_list():
     global oval_cords, f_name, o

     file = f_name.get()
     f = open(file,'r')

     oval_cords = f.read()
     oval_cords = ast.literal_eval(oval_cords)

     o = 1
     method()


def get_file():
    global f_name, k, f_name

    random_btn.destroy()
    input_file_btn.destroy()
    choice_label.destroy()
     
    k = tk.Button(text= 'Enter the file name in the input box', command = get_list,  font = ('Courier New',11),
                  fg = 'white', bg = 'black')
    k.place(anchor = 'se' ,relx = 0.5, rely = 0.5)

    f_name = tk.Entry(relief=tk.GROOVE, bg='grey')
    f_name.place(relx = 0.505, rely = 0.47)


def cords():
    global oval_cords,canvas,np

    canvas = create_canvas()
    canvas.update()
    canvas_width = canvas.winfo_width()
    canvas_height = canvas.winfo_height()-100

    pseudo_list = copy.deepcopy(oval_cords)
    while len(pseudo_list) > 0:

        cord = pseudo_list.pop(0)
        x = cord[1][0]
        y = cord[1][1]
        i = cord[0]
        canvas.create_oval(x - 5, y - 5, x + 5, y + 5, fill='blue')
        canvas.create_text(x + 10, y - 10, text=str(np), fill="red", font=("Arial", 10))
        np+=1

    dist_lst_calc()

w_msg = tk.Label(text = 'Welcome to Minimum Spanning Tree Program :) ', bg = 'light blue', 
                fg ='black', font = ('Courier',19), border = 10 , bd =5 )
w_msg.place(relx = 0.28, rely = 0.0)

choice_label = tk.Label(text = 'Choose the way you want to use the program with:', bg = 'black', 
                        fg = 'white', font = ('Courier New',15), relief = 'raised')
choice_label.place(relx = 0.3, rely = 0.4)

random_btn = tk.Button(text='1. Random coordinates', command= get_np, background='black', 
                        fg = 'white',  font = ('Courier New',11), relief = 'raised')
random_btn.place(anchor='se', relx=0.433, rely=0.5)

input_file_btn = tk.Button(text='2. Input coordinates via file', command= get_file , background= 'black', 
                            fg = 'white',  font = ('Courier New',11), relief = 'raised')
input_file_btn.place(anchor='se', relx=0.68, rely=0.5)

exit_b = tk.Button(text = 'EXIT',  font = ('Courier New',9), command = exit, fg = 'white', bg = 'black')
exit_b.place(relx = 0.96, rely = 0.96)

GUI.mainloop()
