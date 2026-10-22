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
1. 
2. 
3. 
4. 
5. 
6. 
7. 
8. 
9.
