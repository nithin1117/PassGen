import secrets
import random
import string
import mysql.connector

store = {}

mydb = mysql.connector.connect(
    host="localhost",
    username="root",
    password="0303",
    database="test"
)

mycursor = mydb.cursor()


def password_gen(u_name):
    password = secrets.choice(string.ascii_lowercase)
    password += secrets.choice(string.ascii_uppercase)
    password += secrets.choice(string.digits)
    password += secrets.choice(string.digits)
    password += secrets.choice(string.hexdigits)
    password += secrets.choice(string.octdigits)
    password += secrets.choice(string.octdigits)
    password += secrets.choice(string.punctuation)

    password = list(password)
    random.shuffle(password)

    store[u_name] = "".join(password)

    return store[u_name]


def username():
    user_id = input("Enter User ID : ")
    user_id = user_id.replace(" ", "_")

    mycursor.execute("select username from storedata;")
    result = mycursor.fetchall()

    myresult = [x[0] for x in result]

    if (user_id not in myresult) and (user_id not in store):
        print("Hello {}!, your password is \"{}\"".format(user_id, password_gen(user_id)))
        print()

    else:
        print("Already Exists!!! Try Again")
        username()
    mydb.commit()

def authenticate_user():
    try:
        sql = "SELECT password FROM storedata WHERE username = %s;"
        adr = tuple([input("Enter username : ")])
        mycursor.execute(sql, adr)
        result = mycursor.fetchone()
        while len(result):
            password = input("Enter password : ")
            if password == result[0]:
                print("Validation Successful!!")
                print()
                return adr
            else:
                print("Try again!!")

    except:
        print("Invalid Username")
        authenticate_user()

def update():
    sql = "UPDATE STOREDATA SET PASSWORD = %s WHERE USERNAME = %s;"
    a = authenticate_user()
    b = input("Enter Password to Update :")
    i = (b,a[0])
    mycursor.execute(sql,i)
    
    mydb.commit()
    
    print("Password Updated Successfully!")
    print()

if __name__ == "__main__":

    try:
        # Create Table
        mycursor.execute("CREATE TABLE STOREDATA (USERNAME VARCHAR (50), PASSWORD VARCHAR(10));")
    except:
        # If table already exists, proceed!
        pass
    sql = "INSERT INTO STOREDATA (USERNAME, PASSWORD) VALUES (%s, %s);"
    choice = '_'
    while choice != '0':
        if choice in '123':
            if choice == '1':
                n = int(input("Enter Number of Users:"))
                for i in range(n):
                    # Password Generator function call
                    username()
                for i in store.items():
                    index = tuple(map(str, i))
                    mycursor.execute(sql, index)
            if choice == '2':
                # Authenticate user function call
                authenticate_user()
            if choice == '3':
                update()

        else:
            print("Welcome, Kindly your choice")
            print("1. Password Generation")
            print("2. Password Validation")
            print("3. Update Password")
            print("0. Exit")
        choice = input("Enter Choice : ")

    # Commit Database
    mydb.commit()
