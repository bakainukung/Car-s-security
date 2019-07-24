from _future_ import print_function
from datetime import datetime
from gpiozero import Buzzer
import serial
import time
import os
import datetime
import sys
import MySQLdb
import webbrowser
import os,re
import sys
buzzer = Buzzer(17)

def getdb():
db = MySQLdb.connect(host="35.240.172.173",user="admin",passwd="1234",db="test") 
cur = db.cursor()
cur.execute("SELECT alert FROM car_information WHERE car_id = 3 ")
for row in cur.fetchall():
alarm = row[0]
db.close()
print (alarm)
return alarm;

def uptxt(rs,noc):
fk = "3"

os.system("sudo cp /home/pi/output2.txt /home/pi/output3.txt") 
file = open('/home/pi/output2.txt', 'r')
file_content = file.read().strip()
file_content = file_content.split(' ')
file.close()

file = open("/home/pi/output3.txt","w")#a=add w=writh
timeA = "14.15"

p=os.popen("sudo python time.py").readlines()
str_pid="".join(p)
xtime=str_pid.split()


#file.write(file_content[0] + " " + file_content[2] + " " + timeA + " " + "Even" + " " + "0" +" " + "null" + " " + "3" )
# file.write(file_content[0] + " " + file_content[2] + " " + xtime[0] + " " + rs + " " + noc +" " + xtime[1] + " " + fk )
file.write(file_content[0] + " " + file_content[2] + " " + xtime[0] + " " + fk + " " + noc +" " + xtime[1] + " " + rs )
file.close()



file = open("/home/pi/output3.txt","r")
file_content = file.read().strip()
file_content = file_content.split(' ')
file.close()
print (file_content) 
db = MySQLdb.connect("35.240.172.173","admin","1234","test")
cursor = db.cursor()
query = "INSERT INTO car_log(LAT, LNG, date, status, notify, time, fk_carlog) VALUES ('"+file_content[0]+"','"+file_content[1]+"','"+file_content[2]+"','"+file_content[6]+"','"+file_content[4]+"','"+file_content[5]+"','"+file_content[3]+"')"
cursor.execute(query)
db.commit()
db.close()

print ("End Up load Texts")
time.sleep(5)
return;
def Line(read):

os.system("curl -X POST -H 'Authorization: Bearer Y8QJkFWehT4hPM7YZ5JlhVxx4GVjk7b6CP8N38wazPD' -F 'message= {}' \https://notify-api.line.me/api/notify".format(read))

#os.system("curl -X POST -H 'Authorization: Bearer Y8QJkFWehT4hPM7YZ5JlhVxx4GVjk7b6CP8N38wazPD' -F 'message= put mess' \https://notify-api.line.me/api/notify")
return;


def event1(tt):
for x in range(0,2):
os.system("fswebcam -r 1280x720 /home/pi/Pictures/SPic%d.jpg"% (x+tt)) 
print("Start up load")
for y in range(0,2):
os.system("gsutil cp /home/pi/Pictures/SPic%d.jpg gs://carsecurity/picture"% (y+tt))
print("End up load")
tt=tt+10
return tt ; 

def gps():
os.system("sudo python /home/pi/gps2-1.py") #gps
return;

if os.geteuid() != 0: 
os.execvp("sudo", ["sudo"] + sys.argv)
os.system("sudo python killp.py") 
ser = serial.Serial('/dev/ttyUSB0',9600)
s = [0]
t=0
x=0
notify = 1 #1 active 0 non active
start = time.time()
end = start + 60
mode = 0 #1 active 0 non active
testTime = datetime.datetime.now()
#read_serial = "test"
buzzer.beep()
#Line(str(testTime))
buzzer.off()
#time.sleep(100000)

while True:
print("Read serial...")
# read_serial = ("HelloWorld")
read_serial = ser.readline()
print (read_serial)
print ("END Read serial\n")
#uptxt(str(read_serial),str(notify))
#time.sleep(1)
if time.time() > end:
end = time.time()+60.0
print("Check GPS******************************************************")
#gps()
#notify = getdb() dont forget open------------------------------------------------------------------------------------

if(read_serial[:1] == "O"):
mode = "1"
buzzer.beep()
time.sleep(1)
buzzer.off()

if(read_serial[:1] == "F"):
mode = "0"
buzzer.beep()
time.sleep(0.5)
buzzer.off()
if(read_serial[:1] == "R"):
buzzer.beep()
time.sleep(2)

os.system("sudo reboot") #gps
buzzer.off()
if(read_serial[:1] == "T"): #test
buzzer.beep()
time.sleep(1)
os.system("TEST Active") #gps
os.system("curl -X POST -H 'Authorization: Bearer Y8QJkFWehT4hPM7YZ5JlhVxx4GVjk7b6CP8N38wazPD' -F 'message= System Active' \https://notify-api.line.me/api/notify")
buzzer.off()
time.sleep(1)
buzzer.beep()
time.sleep(1)
buzzer.off()



if(read_serial[:1] == "E" and mode == "1" and notify == 1 ):
print ("Event Active")
print ("Line Alame")
Line(str(read_serial))
print ("Get alarm DB")
## notify = getdb()
print (notify)
print ("Cap picture ...")
#t = event1(t)#cap
time.sleep(2)
print ("End Cap ...")

#123....


print("Start Up texts.")
# uptxt(str(read_serial),str(notify))
time.sleep(1)
print("End up texts")
#time.sleep(1000)
print("Delay Loop...")
time.sleep(0.1)
