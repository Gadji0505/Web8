// Очистка базы данных
MATCH (n) DETACH DELETE n;

// Создание всей структуры одним CREATE
CREATE 
  (aspirin:Med {id: 1, name: "Aspirin", type: "Slab", dosage: "500 mg"}),
  (ibuprofen:Med {id: 2, name: "Ibuprofen", type: "Slab", dosage: "400 mg"}),
  (amoxicillin:Med {id: 3, name: "Amoxicillin", type: "Antibiotic", dosage: "250 mg"}),
  (symamed:Med {id: 4, name: "Symamed", type: "Antibiotic", dosage: "300 mg"}),
  (gyttalaks:Med {id: 5, name: "Gyttalaks", type: "Slab"}),
  
  (zdorove:Manufacturer {id:1, name:'Zdorove', country:'Russia', date_of_create:'22.08.2005'}),
  (teplo:Manufacturer {id:2, name:'Teplo', country:'Belarus', date_of_create:'01.08.2006'}), 
  (dobro:Manufacturer {id:3, name:'Dobro', country:'Russia', date_of_create:'19.03.1986'}),
  (ozon:Manufacturer {id:4, name:'Ozon', country:'Russia', date_of_create:'15.07.2018'}),
  (kliniks:Manufacturer {id:5, name:'Kliniks', country:'Belarus', date_of_create:'25.05.1995'}),
  
  (aibolit:Doctor {id:1, name:'Aibolit', qualife:'Terapevt', exp_by_years:5}),
  (mechnikov:Doctor {id:2, name:'Mechnikov', qualife:'Surgeon', exp_by_years:10}),
  (archimed:Doctor {id:3, name:'Archimed', qualife:'Lor', exp_by_years:10}),
  (sklifosovskii:Doctor {id:4, name:'Sklifosovskii', qualife:'Hiryrg', exp_by_years:27}),
  (dykin:Doctor {id:5, name:'Dykin', qualife:'Terapevt', exp_by_years:34}),
  
  (ashot:Patient {id:1, name:'Ashot', age:19, sex:'male', disease:'temperature'}),
  (andrey:Patient {id:2, name:'Andrey', age:15, sex:'male', disease:'headache'}),
  (anna:Patient {id:3, name:'Anna', age:35, sex:'female', disease:'nausea'}),
  (masha:Patient {id:4, name:'Masha', age:23, sex:'female', disease:'temperature'}),
  (gadgi:Patient {id:5, name:'Gadgi', age:18, sex:'male', disease:'temperature'}),
  
  (patient_1:Recept {id:1, patient_id:1, doctor_id:1, pharmacy_id:1, date_of_start:'01.01.2025', date_of_end:'01.01.2026'}), 
  (patient_2:Recept {id:2, patient_id:2, doctor_id:2, pharmacy_id:2, date_of_start:'10.10.2004', date_of_end:'10.10.2006'}), 
  (patient_3:Recept {id:3, patient_id:3, doctor_id:3, pharmacy_id:3, date_of_start:'05.05.1999', date_of_end:'05.05.2001'}), 
  (patient_4:Recept {id:4, patient_id:4, doctor_id:4, pharmacy_id:4, date_of_start:'01.01.2025', date_of_end:'01.01.2026'}), 
  (patient_5:Recept {id:5, patient_id:5, doctor_id:5, pharmacy_id:5, date_of_start:'10.10.2002', date_of_end:'10.03.2003'}),
  
  (patient_1)-[:HAS]->(aspirin),
  (patient_2)-[:HAS]->(ibuprofen),
  (patient_3)-[:HAS]->(amoxicillin),
  (patient_4)-[:HAS]->(symamed),
  (patient_5)-[:HAS]->(gyttalaks),
  
  (aibolit)-[:TREATS]->(ashot),
  (mechnikov)-[:TREATS]->(andrey),
  (archimed)-[:TREATS]->(anna),
  (sklifosovskii)-[:TREATS]->(masha),
  (dykin)-[:TREATS]->(gadgi),
  
  (ashot)-[:HAS_PRESCRIPTION]->(patient_1),
  (andrey)-[:HAS_PRESCRIPTION]->(patient_2),
  (anna)-[:HAS_PRESCRIPTION]->(patient_3),
  (masha)-[:HAS_PRESCRIPTION]->(patient_4),
  (gadgi)-[:HAS_PRESCRIPTION]->(patient_5),
  
  (zdorove)-[:PRODUCES]->(aspirin),
  (teplo)-[:PRODUCES]->(ibuprofen),
  (dobro)-[:PRODUCES]->(amoxicillin),
  (ozon)-[:PRODUCES]->(symamed),
  (kliniks)-[:PRODUCES]->(gyttalaks);

// Запросы для выполнения заданий:

// 1. Вывод докторов и их пациентов
MATCH (d:Doctor)-[:TREATS]->(p:Patient)
RETURN d.name AS Doctor, p.name AS Patient;

// 2. Количество уникальных пациентов у каждого доктора
MATCH (d:Doctor)-[:TREATS]->(p:Patient)
WITH d, COUNT(DISTINCT p) AS patientCount
RETURN d.name AS DoctorName, patientCount;

// 3. Удаление заболевания у пациента (например, у пациента Ashot)
MATCH (p:Patient {name: 'Ashot'})
SET p.disease = NULL
RETURN p;

// 4. Поиск пациентов с определенным заболеванием (например, 'temperature')
MATCH (p:Patient)
WHERE p.disease CONTAINS 'temperature'
RETURN p.name AS PatientName, p.disease;

// 5. Доктора и список их пациентов
MATCH (d:Doctor)-[:TREATS]->(p:Patient)
WITH d, COLLECT(p.name) AS patients
RETURN d.name AS Doctor, patients;

//просто задание удалить так чтобы у какаого нибудь пациента не было заболевания(на паре)
//работать с помощью команд удаления
//находим все узлы с меткой пациент
//null отсутствующее или не заданное значение
//ВЫЖНО!!! Поиск в подстраке

//ДЗ (Работаем с той же базой) Потом будет провекрка дз
разобраться написать запросы по лабе 3, с гугл дисков взять что нужно
