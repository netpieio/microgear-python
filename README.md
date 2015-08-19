#Microgear-python
-----------
microgear- python คือ client library ภาษา Python  ที่ทำหน้าที่เป็นตัวกลางในการเชื่อมโยง application code หรือ hardware เข้ากับบริการของ netpie platform เพื่อการพัฒนา IOT application รายละเอียดเกี่ยวกับ netpie platform สามารถศึกษาได้จาก http://netpie.io



##การติดตั้ง
-----------
pip install microgear



##ตัวอย่างการเรียกใช้งาน
-----------
```python
import microgear.client as client
import time

gearkey = <gearkey>
gearsecret =  <gearsecret>
appid = <appid>

microgear.create(gearkey,gearsecret,appid,{'debugmode': True})

def connection():
	print "Now I am connected with netpie"

def subscription(topic,message):
	client.chat("nobita","Hello world. "+str(int(time.time())))
	print topic+" "+message

client.setname("doraemon", subscription)
client.on_connect = connection
client.subscribe("/mails", subscription)

client.connect()
```



##การใช้งาน library
------------
**microgear.create(*gearkey*,*gearsecret*,*appid*):**

arguments

 * *gearkey* `string` - เป็น key สำหรับ gear ที่จะรัน ใช้ในการอ้างอิงตัวตนของ gear
 * *gearsecret* `string` - เป็น secret ของ key ซึ่งจะใช้ประกอบในกระบวนการยืนยันตัวตน
 * *appid* `string` - กลุ่มของ application ที่ microgear จะทำการเชื่อมต่อ

```python
gearkey = <gearkey>
gearsecret =  <gearsecret>
appid = <appid>

microgear.create(gearkey,gearsecret,appid)
```




##Microgear
---------------

**microgear.connect():** การเชื่อมต่อ microgear

```python
microgear.connect();
```







**microgear.setname(*gearname*, *callback*):** microgear สามารถตั้งชื่อตัวเองได้
ซึ่งสามารถใช้เป็นชื่อเรียกในการใช้ฟังก์ชั่น chat() ซึ่งเมื่อได้รับข้อความข้อความจะถูกส่งเข้า function
callback ที่ระบุไว้

argument

* *gearname* `string` - ชื่อของ microgear นี้
* *callback* `function` - ฟังก์ชั่น ที่จะทำงานเมื่อได้รับข้อความ โดยฟังก์ชั่นนี้จะรับ parameter 2 ตัวคือ
    * *topic* - ชื่อ topic ที่ได้รับข้อความนี้
    * *message* - ข้อความที่ได้รับ








```python
def callback(topoic, message):
	print "Yes, I hear you."
microgear.setname("python", callback);
```

**microgear.chat(*gearname*, *message*):** การส่งข้อความโดยระบุ gearname และข้อความที่ต้องการส่ง

arguments

* *gearname* `string` - ชื่อของ microgear นี้
* *message* `string` – ข้อความ

```python
microgear.chat("html","hello from python");
```







**microgear.publish(*topic*, *message*):** ในการณีที่ต้องการส่งข้อความแบบไม่เจาะจงผู้รับ สามารถใช้ฟังชั่น publish ไปยัง topic ที่กำหนดได้ ซึ่งจะมีแต่ microgear ที่ subscribe topoic นี้เท่านั้น ที่จะได้รับข้อความ

arguments

* *topic* `string` - ชื่อของ topic ที่ต้องการจะส่งข้อความไปถึง
* *message* `string` – ข้อความ

```python
microgear.publish("/outdoor/temp","28.5");
```




**microgear.subscribe(*topic*, *callback*)** microgear อาจจะมีความสนใจใน topic
ใดเป็นการเฉพาะ เราสามารถใช้ฟังก์ชั่น subscribe() ในการบอกรับ message ของ topic นั้นได้
 และฟังก์ชั่น callback จะถูกเรียกเมื่อได้รับข้อความ ตาม topic ที่ระบุ

argument

* *topic* `string` - ชื่อของ topic ที่ความสนใจ
* *callback* `function` - ฟังก์ชั่น ที่จะทำงานเมื่อได้รับข้อความ โดยฟังก์ชั่นนี้จะรับ parameter 2 ตัวคือ
    * *topic* - ชื่อ topic ที่ได้รับข้อความนี้
    * *message* - ข้อความที่ได้รับ



```python
def callback(topic, message):
	print "I got it: "+ message
microgear.subscribe("/outdoor/temp", callback);
```



##Event
---------------
application ที่รันบน microgear จะมีการทำงานในแบบ event driven คือเป็นการทำงานตอบสนองต่อ event ต่างๆ ด้วยการเขียน callback function ขึ้นมารองรับในลักษณะๆดังต่อไปนี้

**microgear.on_connect**  เกิดขึ้นเมื่อ microgear library เชื่อมต่อกับ platform สำเร็จ

ค่าที่ set

* *callback* `function` - ฟังก์ชั่นที่จะทำงาน เมื่อมีการ connect


```python
def callback_connect() :
	print "Now I am connected with netpie"
microgear.on_ connect = callback_connect
```




**microgear.on_disconnect** เกิดขึ้นเมื่อ microgear library ตัดการเชื่อมต่อกับ platform

ค่าที่ set


* *callback* `function` - callback function


```python
def callback_disconnect() :
	pritnt "Disconnected”
microgear.on_disconnect = callback_disconnect

```




**microgear.on_present** event นี้จะเกิดขึ้นเมื่อมี microgear ใน appid เดียวกัน online เข้ามาเชื่อมต่อ netpie

ค่าที่ set


* *callback* `function` - จะทำงานเมื่อเกิดเหตุการณ์นี้ โดยจะรับค่า parameter คือ
     * *gearkey* - ระบุค่าของ gearkey ที่เกี่ยวข้องกับเหตุการณ์นี้


```python
def callback_present(gearkey) :
	pritnt gearkey+" become online."
microgear.on_present = callback_present

```




**microgear.on_present** event นี้จะเกิดขึ้นเมื่อมี microgear ใน appid เดียวกัน offline หายไป

ค่าที่ set


* *callback* `function` - จะทำงานเมื่อเกิดเหตุการณ์นี้ โดยจะรับค่า parameter คือ
    * *gearkey* - ระบุค่าของ gearkey ที่เกี่ยวข้องกับเหตุการณ์นี้


```python
def callback_absent(gearkey) :
	pritnt gearkey+" become offline."
microgear.on_absent = callback_absent

```
