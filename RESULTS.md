# K6 lab results 
  
## Part 0
- Step 0.1, http_req_duration: avg=267.89ms min=267.89ms med=267.89ms max=267.89ms p(90)=267.89ms p(95)=267.89ms
- Step 0.2, requests, rate and p(95), and why the rate stays below 5: requests=80 // rate = 3.77501/s and p(95) = 2.06s //The rate stays below 5 เนื่องจากหนึ่งรอบมีเวลารอ response และมี sleep(1) ด้วย จึงทำได้ไม่ถึง 1 request ต่อวินาทีต่อ VU
- Step 0.6, exit code of the normal run and of the run with P95=300:  normal run = 0 // run with P95=300 = 99
- Step 0.7, rules that passed, p(95), p(99), exit code after your change: rules that passed=rate>0.99,p(95)<1000,p(99)<1500,rate<0.01 // p(95) = 390.42ms // p(99) = 413.93ms // exit code after your change = 99
- Step 0.8, what was wrong, and how the checks showed it: 
what was wrong = ปัญหาคือส่งชื่อ `pepperoni` และ `knife` ไม่ตรงกับค่าที่ API ใช้จริง จึงแก้เป็น `Pepperoni` และ `Knife`
how the checks showed it = การทดสอบหลาย request พบปัญหา เพราะระบบสุ่ม pizza หลายแบบ จึงมีโอกาสเจอ pizza ที่มี Pepperoni หรือใช้ Knife มากขึ้น ส่วนการทดสอบมือครั้งเดียวอาจบังเอิญได้ผลลัพธ์ที่ผ่าน แม้ request จะผิด
  
## Part A results 
| Step | Test type    | Users | Req/s | p(95) | Max | Failed | Checks | 
| ---- | ------------ | ----- | ----- | ----- | --- | ------ | ------ | 
| A1   | Smoke        | 1     |0.470014|   390 ms |  415 ms  |  0.00% |  100.00%  | 
| A2   | Average load | 3     | 1.26   |   412 ms |  786 ms  |  0.00% |  100.00%  | 
| A3   | Stress       | 5     | 2.11   |   387 ms |  529 ms  |  0.00% |  100.00%  | 
| A4   | Spike        | 5     |1.021802|   388 ms |  457 ms  |  0.00% |  100.00%  |  
| A5   | Soak         | 2     |0.954508|   400 ms |  1.66 s  |  0.00% |  100.00%  |
  
## Part B 
1. ตารางผล A1-A5 กรอกครบแล้ว
2. แม้มี 5 VUs แต่แต่ละรอบต้องรอ response และมี sleep() จึงส่ง request ได้ไม่ถึง 5 ครั้งต่อวินาที
3. Response time ไม่เพิ่มขึ้น เพราะ A2 มี p(95) 412 ms แต่ A3 มี p(95) 387 ms ระบบยังทำงานอยู่ใน operational envelope และผลอาจต่างกันจากความผันผวนของเครือข่าย
4. ตอน spike ค่า p(95) เพิ่มขึ้นเล็กน้อย แล้วกลับมาใกล้ค่าปกติภายในประมาณ 5-10 วินาทีหลังลดจำนวนผู้ใช้กลับเหลือ 1 คน
5. Error rate ไม่เพิ่มขึ้น เพราะยังเป็น 0.00% ตลอดการทดสอบ แม้มีช่วงที่ response time ช้าลงชั่วคราว แต่ไม่ได้แย่ลงต่อเนื่องจนจบการทดสอบ Soak test หลายชั่วโมงอาจพบปัญหา memory leak ได้
6. p(95) แสดงประสบการณ์ของผู้ใช้ส่วนใหญ่และเห็น request ที่ช้าได้ดีกว่าค่าเฉลี่ย ซึ่งอาจถูกทำให้ดูดีเกินจริง
7. ผลต่างกันได้เพราะความเร็วและความหน่วงของอินเทอร์เน็ตไม่คงที่ และ QuickPizza เป็นเว็บสาธารณะที่มีผู้ใช้อื่นร่วมใช้งาน
8. เพราะอาจทำให้เว็บช้า ล่ม หรือรบกวนผู้ใช้งานจริง และเป็นการทดสอบโดยไม่ได้รับอนุญาต
9. ข้อกำหนดแบบไม่ชัดเจน: แอปสั่งอาหารต้องตอบสนองเร็ว

   ข้อกำหนดที่ทดสอบได้: เมื่อมีผู้ใช้ 10 คนเป็นเวลา 1 นาที 95% ของ request ต้องตอบภายใน 1 วินาที, 99% ภายใน 1.5 วินาที และ request ที่ล้มเหลวน้อยกว่า 1%

   thresholds:
   http_req_failed: ['rate<0.01']
   http_req_duration: ['p(95)<1000', 'p(99)<1500']