def show_values(usernames: list,passwords: list):
   
    for i in range(len(usernames)):
        print(f"{usernames[i]} : {passwords[i]}")
        
def generate_password() -> str:
    
    import random
    str0=".,:;!_*-+()/#¤%&"
    str1 = '0123456789'
    str2 = 'qwertyuiopasdfghjklzxcvbnm'
    str3 = str2.upper()
    print(str3) # 'QWERTYUIOPASDFGHJKLZXCVBNM'
    str4 = str0+str1+str2+str3
    print(str4)
    ls = list(str4)
    print(ls)
    random.shuffle(ls)
    print(ls)
    psword = ''.join([random.choice(ls) for x in range(12)])
    print(psword)
    return psword
    
def show_menu() -> str:
    menu_list = ["Авторизироваться", "Зарегестрироваться","Закончить работу"]
    m_counter = 1

    for i in menu_list:
        print(f"{m_counter}.{i}")
        m_counter += 1

    while True:
        choise = input_int("Выберите действие: ")
        if choise not in range(len(menu_list)) or choise == 3:
            return "Закончить работу"
        else:
            return menu_list[choise-1]
  
def input_int(message: str) -> int:
    
    while True:
        dig = input(message)
        try:
            dig = int(dig)
            return dig
        except:
            print("Not a digit! Press \'Q\' to exit")
def autorization(usernames: list,passwords: list) -> bool:
    while True:

        username_input = input("Введите имя пользователя: ")
        
        if usernames.count(username_input) < 1:
            print("Такого пользовтеля не существует")
            if input_int("1.Попробовать еще раз\n2.Выйти в главное меню\n") == 2:
                return False
        else:
            username_index = usernames.index(username_input) 
            break
    
    while True:
       
        pass_input = input(f"Введите пароль для {usernames[username_index]}: ")
       
        if pass_input == password_list[username_index]:
            print("Пароль правильный. Вход выполнен успешно")
            return True
        else:
            print("Неправильный пароль!")
            if input_int("1.Попробовать еще раз\n2.Выйти в главное меню\n") == 2:
                return False
def check_new_password(password:str) -> bool:
    
    special_chars=".,:;!_*-+()/#¤%&"
    digits = 0
    s_letter = 0
    l_letter = 0
    s_char = 0

    for i in password:
        if i.isdigit():
            digits += 1
        elif i.islower():
            s_letter +=1
        elif i.isupper():
            l_letter += 1
        else:
            for c in special_chars:
                if i == c:
                    s_char += 1
    
    conf = True
    if digits < 1:
        print("Пароль должен содержать хотя бы одну цифру!")
        conf = False
    if s_letter < 1:
        print("Пароль должен содержать хотя бы одну маленькую букву!")
        conf = False
    if l_letter < 1:
        print("Пароль должен содержать хотя бы одну заглавную букву!")
        conf = False
    if s_char < 1:
        print("Пароль должен содержать хотя бы один специальный символ! (.,:;!_*-+()/#¤%&)")
        conf = False

    return conf

def registration(usernames: list,passwords: list) -> list:
    
    new_username = ""
    new_password = ""
    while True:
        
        new_username = input("Введите новое имя пользовтеля: ")
     
        if usernames.count(new_username) > 0:
            print("Имя пользователя уже существует")
            if input_int("1.Попробовать еще раз\n2.Выйти в главное меню\n") == 2:
                return ["",""]
        else:
            print(f"Привет, {new_username}!")
            break
   
    x = input_int("1.Сгенерировать пароль автоматически\n2.Ввести пароль вручную\n3.Выйти в главное меню\n")
    if x == 3:
        return ["",""]
    elif x == 1: 
        new_password = generate_password()
    elif x == 2: 
        while True:
            print("Придумай пароль, с данными требованиями:\nцифра\nмаленькая буква\nзаглавная буква\nспец. символ(.,:;!_*-+()/#¤%&)")
            new_password = input()
            if check_new_password(new_password) == False:
                if input_int("1.Попробовать еще раз\n2.Выйти в главное меню\n") == 2:
                    return ["",""]
            else:
                break

    return [new_username,new_password]

username_list = ["user"]
password_list = ["user2002"]

while True: 
    menu = show_menu() 
    if menu == "Авторизироваться":
        logged_in = autorization(username_list,password_list)
        if logged_in:
            show_values(username_list,password_list)
    elif menu == "Зарегестрироваться":
        new_user = registration(username_list,password_list)
        new_user_name = new_user[0]
        new_user_pass = new_user[1]
        if new_user_name != "":  
            username_list.append(new_user_name)
            password_list.append(new_user_pass)
            print(f"Пользователь {username_list[-1]} успешно добавлен.")
        else:
            print("Отмена добавления пользователя")

    elif menu == "Закончить работу":
        break
