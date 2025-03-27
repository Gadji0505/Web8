Создание бд
// Коллекция болезней
db.disease.insertMany([
 { _id: ObjectId(), name: "Грипп", type: "Вирусная инфекция", severity: "Средняя", contagious: true },
 { _id: ObjectId(), name: "Пневмония", type: "Бактериальная инфекция", severity: "Высокая", contagious: false },
 { _id: ObjectId(), name: "Гипертония", type: "Сердечно-сосудистое заболевание", severity: "Средняя", contagious: false },
 { _id: ObjectId(), name: "Диабет", type: "Обменное заболевание", severity: "Высокая", contagious: false },
 { _id: ObjectId(), name: "Астма", type: "Респираторное заболевание", severity: "Средняя", contagious: false },
 { _id: ObjectId(), name: "Аллергия", type: "Иммунологическое заболевание", severity: "Низкая", contagious: false },
 { _id: ObjectId(), name: "Гастрит", type: "Заболевание ЖКТ", severity: "Средняя", contagious: false }
])
 
// Коллекция симптомов
db.symptom.insertMany([
 { _id: ObjectId(), name: "Кашель", type: "Респираторный", severity: "Средняя" },
 { _id: ObjectId(), name: "Лихорадка", type: "Инфекционный", severity: "Высокая" },
 { _id: ObjectId(), name: "Головная боль", type: "Неврологический", severity: "Низкая" },
 { _id: ObjectId(), name: "Боль в горле", type: "Респираторный", severity: "Средняя" },
 { _id: ObjectId(), name: "Насморк", type: "Респираторный", severity: "Низкая" },
 { _id: ObjectId(), name: "Высокое давление", type: "Кардиологический", severity: "Высокая" },
 { _id: ObjectId(), name: "Тошнота", type: "Гастроэнтерологический", severity: "Средняя" }
])
 
 
// Коллекция врачей
db.doctor.insertMany([
 { _id: ObjectId(), name: "Иванов Иван", specialty: "Терапевт", experience: 10 },
 { _id: ObjectId(), name: "Петрова Анна", specialty: "Кардиолог", experience: 5 },
 { _id: ObjectId(), name: "Сидоров Петр", specialty: "Пульмонолог", experience: 15 },
 { _id: ObjectId(), name: "Козлова Мария", specialty: "Аллерголог", experience: 8 },
 { _id: ObjectId(), name: "Смирнов Дмитрий", specialty: "Гастроэнтеролог", experience: 12 },
 { _id: ObjectId(), name: "Кузнецова Елена", specialty: "Инфекционист", experience: 7 },
 { _id: ObjectId(), name: "Федорова Ольга", specialty: "Эндокринолог", experience: 10 }
])
 
// Коллекция назначений - упрощенная без внешних ключей
db.prescriptionNew.insertMany([
 {  _id: ObjectId(), medication: "Парацетамол", dosage: "2 таблетки 3 раза в день" },
 {  _id: ObjectId(), medication: "Каптоприл", dosage: "1 таблетка 2 раза в день" },
 {  _id: ObjectId(), medication: "Амоксициллин", dosage: "500 мг 3 раза в день" },
 {  _id: ObjectId(), medication: "Супрастин", dosage: "1 таблетка 3 раза в день" },
 {  _id: ObjectId(), medication: "Омепразол", dosage: "20 мг 1 раз в день" },
 {  _id: ObjectId(), medication: "Оселтамивир", dosage: "75 мг 2 раза в день" },
 {  _id: ObjectId(), medication: "Инсулин", dosage: "По назначению врача" }
])
 
// Коллекция людей - упрощенная без внешних ключей
db.peopleNew.insertMany([
 {  _id: ObjectId(), name: "Петр Петров", age: 30},
 {  _id: ObjectId(), name: "Мария Иванова", age: 45},
 {  _id: ObjectId(), name: "Иван Сидоров", age: 25},
 {  _id: ObjectId(), name: "Анна Кузнецова", age: 50},
 {  _id: ObjectId(), name: "Дмитрий Смирнов", age: 35},
 {  _id: ObjectId(), name: "Елена Федорова", age: 60},
 { _id: ObjectId(), name: "Ольга Козлова", age: 28}
]);
// Получаем идентификаторы симптомов
const coughId = db.symptom.findOne({ name: "Кашель" })._id;
const feverId = db.symptom.findOne({ name: "Лихорадка" })._id;
const headacheId = db.symptom.findOne({ name: "Головная боль" })._id;
const soreThroatId = db.symptom.findOne({ name: "Боль в горле" })._id;
const runnyNoseId = db.symptom.findOne({ name: "Насморк" })._id;
const highPressureId = db.symptom.findOne({ name: "Высокое давление" })._id;
const nauseaId = db.symptom.findOne({ name: "Тошнота" })._id;
 
// Получаем идентификаторы назначений
const paracetamolId = db.prescriptionNew.findOne({ medication: "Парацетамол" })._id;
const captoprilId = db.prescriptionNew.findOne({ medication: "Каптоприл" })._id;
const amoxicillinId = db.prescriptionNew.findOne({ medication: "Амоксициллин" })._id;
const suprastinId = db.prescriptionNew.findOne({ medication: "Супрастин" })._id;
const omeprazoleId = db.prescriptionNew.findOne({ medication: "Омепразол" })._id;
const oseltamivirId = db.prescriptionNew.findOne({ medication: "Оселтамивир" })._id;
const insulinId = db.prescriptionNew.findOne({ medication: "Инсулин" })._id;
 
// Получаем идентификаторы болезней
const fluId = db.disease.findOne({ name: "Грипп" })._id;
const pneumoniaId = db.disease.findOne({ name: "Пневмония" })._id;
const hypertensionId = db.disease.findOne({ name: "Гипертония" })._id;
const diabetesId = db.disease.findOne({ name: "Диабет" })._id;
const asthmaId = db.disease.findOne({ name: "Астма" })._id;
const allergyId = db.disease.findOne({ name: "Аллергия" })._id;
const gastritisId = db.disease.findOne({ name: "Гастрит" })._id;
 
// Получаем идентификаторы врачей
const therapistId = db.doctor.findOne({ name: "Иванов Иван" })._id;
const cardiologistId = db.doctor.findOne({ name: "Петрова Анна" })._id;
const pulmonologistId = db.doctor.findOne({ name: "Сидоров Петр" })._id;
const allergistId = db.doctor.findOne({ name: "Козлова Мария" })._id;
const gastroenterologistId = db.doctor.findOne({ name: "Смирнов Дмитрий" })._id;
const infectiousDiseaseId = db.doctor.findOne({ name: "Кузнецова Елена" })._id;
const endocrinologistId = db.doctor.findOne({ name: "Федорова Ольга" })._id;
 
// Получаем идентификаторы людей
const petrId = db.peopleNew.findOne({ name: "Петр Петров" })._id;
const mariaId = db.peopleNew.findOne({ name: "Мария Иванова" })._id;
const ivanId = db.peopleNew.findOne({ name: "Иван Сидоров" })._id;
const annaId = db.peopleNew.findOne({ name: "Анна Кузнецова" })._id;
const dmitryId = db.peopleNew.findOne({ name: "Дмитрий Смирнов" })._id;
const elenaId = db.peopleNew.findOne({ name: "Елена Федорова" })._id;
const olgaId = db.peopleNew.findOne({ name: "Ольга Козлова" })._id;
 
// Обновляем коллекцию disease (добавляем симптомы и назначения)
db.disease.updateMany(
   {},
   [
       { $set: {
           symptoms: {
               $switch: {
                   branches: [
                       { case: { $eq: ["$name", "Грипп"] }, then: [coughId, feverId, soreThroatId, runnyNoseId] },
                       { case: { $eq: ["$name", "Пневмония"] }, then: [coughId, feverId] },
                       { case: { $eq: ["$name", "Гипертония"] }, then: [highPressureId, headacheId] },
                       { case: { $eq: ["$name", "Диабет"] }, then: [] },
                       { case: { $eq: ["$name", "Астма"] }, then: [coughId] },
                       { case: { $eq: ["$name", "Аллергия"] }, then: [runnyNoseId] },
                       { case: { $eq: ["$name", "Гастрит"] }, then: [nauseaId] }
                   ],
                   default: []
               }
           },
           prescriptions: {
               $switch: {
                   branches: [
                       { case: { $eq: ["$name", "Грипп"] }, then: [paracetamolId, oseltamivirId] },
                       { case: { $eq: ["$name", "Пневмония"] }, then: [amoxicillinId] },
                       { case: { $eq: ["$name", "Гипертония"] }, then: [captoprilId] },
                       { case: { $eq: ["$name", "Диабет"] }, then: [insulinId] },
                       { case: { $eq: ["$name", "Астма"] }, then: [] },
                       { case: { $eq: ["$name", "Аллергия"] }, then: [suprastinId] },
                       { case: { $eq: ["$name", "Гастрит"] }, then: [omeprazoleId] }
                   ],
                   default: []
               }
           }
       }}
   ]
);
 
// Обновляем коллекцию peopleNew (добавляем болезни и врача)
db.peopleNew.updateMany(
   {},
   [
       { $set: {
           diseases: {
               $switch: {
                   branches: [
                       { case: { $eq: ["$name", "Петр Петров"] }, then: [fluId] },
                       { case: { $eq: ["$name", "Мария Иванова"] }, then: [hypertensionId] },
                       { case: { $eq: ["$name", "Иван Сидоров"] }, then: [pneumoniaId] },
                       { case: { $eq: ["$name", "Анна Кузнецова"] }, then: [diabetesId] },
                       { case: { $eq: ["$name", "Дмитрий Смирнов"] }, then: [asthmaId] },
                       { case: { $eq: ["$name", "Елена Федорова"] }, then: [gastritisId] },
                       { case: { $eq: ["$name", "Ольга Козлова"] }, then: [allergyId] }
                   ],
                   default: []
               }
           },
           doctor: {
               $switch: {
                   branches: [
                       { case: { $eq: ["$name", "Петр Петров"] }, then: infectiousDiseaseId },
                       { case: { $eq: ["$name", "Мария Иванова"] }, then: cardiologistId },
                       { case: { $eq: ["$name", "Иван Сидоров"] }, then: pulmonologistId },
                       { case: { $eq: ["$name", "Анна Кузнецова"] }, then: endocrinologistId },
                       { case: { $eq: ["$name", "Дмитрий Смирнов"] }, then: pulmonologistId },
                       { case: { $eq: ["$name", "Елена Федорова"] }, then: gastroenterologistId },
                       { case: { $eq: ["$name", "Ольга Козлова"] }, then: allergistId }
                   ],
                   default: null
               }
           }
       }}
   ]
);
 
// Обновляем коллекцию prescriptionNew (добавляем связь с человеком)
db.prescriptionNew.updateMany(
   {},
   [
       { $set: {
           person: {
               $switch: {
                   branches: [
                       { case: { $eq: ["$medication", "Парацетамол"] }, then: petrId },
                       { case: { $eq: ["$medication", "Каптоприл"] }, then: mariaId },
                       { case: { $eq: ["$medication", "Амоксициллин"] }, then: ivanId },
                       { case: { $eq: ["$medication", "Инсулин"] }, then: annaId },
                       { case: { $eq: ["$medication", "Супрастин"] }, then: olgaId },
                       { case: { $eq: ["$medication", "Омепразол"] }, then: elenaId },
                       { case: { $eq: ["$medication", "Оселтамивир"] }, then: petrId }
                   ],
                   default: null
               }
           }
       }}
   ]
);
 
// Проверка результата
print("Связи успешно созданы. Пример данных:");
 
// Проверка коллекции disease
print("Коллекция disease:");
db.disease.find().forEach(doc => {
   print(`Болезнь: ${doc.name}, Симптомы: ${doc.symptoms}, Назначения: ${doc.prescriptions}`);
});
 
// Проверка коллекции peopleNew
print("\nКоллекция peopleNew:");
db.peopleNew.find().forEach(doc => {
   print(`Пациент: ${doc.name}, Болезни: ${doc.diseases}, Врач: ${doc.doctor}`);
});
 
// Проверка коллекции prescriptionNew
print("\nКоллекция prescriptionNew:");
db.prescriptionNew.find().forEach(doc => {
   print(`Лекарство: ${doc.medication}, Пациент: ${doc.person}`);
});








Максим, [27 мар. 2025 в 11:17]
1 задание
db.peopleNew2.aggregate([
  {
    $match: {
      diseaseN: { $exists: true, $ne: [] }
    }
  },
  {
    $lookup: {
      from: "disease2", localField: "diseaseN", foreignField: "_id", as: "diseases_info"
    }
  },
  {
    $project: {
      _id: 0, name: 1, age: 1, diseases: "$diseases_info.name" // Извлекаем только названия болезней
    }
  },
  {
    $sort: { age: -1 } // Сортировка по возрасту (по убыванию)
  }
])
///////////////////////////////////



2 задание
db.peopleNew2.aggregate([
  { $unwind: "$diseaseN" },
  {
    $group: {
      _id: "$diseaseN", patientCount: { $sum: 1 }
    }
  },
  {
    $lookup: {
      from: "disease2", localField: "_id", foreignField: "_id", as: "diseaseInfo"
    }
  },
  { $unwind: "$diseaseInfo" },
  {
    $project: {
      _id: 0, disease: "$diseaseInfo.name", patientCount: 1
    }
  },
  { $sort: { patientCount: -1 } }
])
//////////////////////////////////////




3 задание
db.peopleNew2.aggregate([
  {
    $lookup: {
      from: "disease2", localField: "diseaseN", foreignField: "_id", as: "diseases"
    }
  },
  {
    $match: {
      "diseases.name": "Грипп"
    }
  },
  {
    $sort: { age: 1 }
  },
  {
    $project: {
      _id: 0, name: 1, age: 1
    }
  }
])
////////////////////////////////




4 задание
db.prescriptionNew2.aggregate([
  {
    $group: {
      _id: "$medication", totalPrescriptions: { $sum: 1 }, uniqueDoctors: { $addToSet: "$doctorN" }, uniquePatients: { $addToSet: "$patientN" }
    }
  },
  {
    $addFields: {
      doctorCount: { $size: "$uniqueDoctors" }, patientCount: { $size: "$uniquePatients" }
    }
  },
  { $sort: { totalPrescriptions: -1 } },
  {
    $project: {
      _id: 0, medication: "$_id", totalPrescriptions: 1, doctorCount: 1, patientCount: 1
    }
  }
])
/////////////////////////////////




5 задание 
db.doctor2.aggregate([
  {
    $sort: {
      specialty: 1, experience: -1
    }
  },
  {
    $group: {
      _id: "$specialty", topDoctor: { $first: "$$ROOT" }
    }
  },
  {
    $project: {
      _id: 0, specialty: "$_id", doctorName: "$topDoctor.name", experience: "$topDoctor.experience"
    }
  },
  { $sort: { specialty: 1 } }
])
////////////////////////////////




6 задание
db.peopleNew2.aggregate([
  { $unwind: "$diseaseN" },
  {
    $lookup: {
      from: "disease2", localField: "diseaseN", foreignField: "_id", as: "diseaseInfo"
    }
  },
  { $unwind: "$diseaseInfo" },
  {
    $group: {
      _id: "$diseaseInfo.name", averageAge: { $avg: "$age" }, patientCount: { $sum: 1 }, severity: { $first: "$diseaseInfo.severity" }
    }
  },
  { $sort: { "_id": 1 } },
  {
    $project: {
      _id: 0,
      diseaseName: "$_id", averageAge: { $round: ["$averageAge", 1] }, patientCount: 1, severity: 1
    }
  }
])
////////////////////////////////




7 задание
db.peopleNew2.aggregate([
  { $unwind: "$diseaseN" },
  {
    $lookup: {
      from: "disease2", localField: "diseaseN", foreignField: "_id", as: "diseaseInfo"
    }
  },
  { $unwind: "$diseaseInfo" },
  {
    $group: {
      _id: "$diseaseInfo.name", patientCount: { $sum: 1 }, severity: { $first: "$diseaseInfo.severity" }
    }
  },
  { $sort: { patientCount: -1 } },
  { $limit: 3 },
  {
    $project: {
      _id: 0, diseaseName: "$_id", patientCount: 1, severity: 1
    }
  }
])
///////////////////////////
