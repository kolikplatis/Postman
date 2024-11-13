1) Необходимо залогиниться
POST http://162.55.220.72:5005/login
login : str (кроме /)
password : str
Приходящий токен необходимо передать во все остальные запросы. Дальше все запросы требуют наличие токена.

        let jsonData = pm.response.json();
        pm.environment.set("token", jsonData.token);  
--- 

2) http://162.55.220.72:5005/user_info
req. (RAW JSON) POST
age: int
salary: int
name: str
auth_token
---

3) http://162.55.220.72:5005/new_data
req. POST
age: int
salary: int
name: str
auth_token

Resp.
{'name':name,
  'age': int(age),
  'salary': [salary, str(salary*2), str(salary*3)]}  

        // 1) Статус код 200
        pm.test("Status code is 200", function () {
            pm.response.to.have.status(200);
        });

        // 2) Проверка структуры json в ответе.
        let jsonData = pm.response.json();
        pm.test("Check schema", function () {
            pm.expect(jsonData).to.have.all.keys("age", "name", "salary");
        });

        // 3) В ответе указаны коэффициенты умножения salary, напишите тесты по проверке правильности результата перемножения на коэффициент.
        let reqData = pm.request.body.formdata;
        let salaryX2 = pm.request.body.formdata.get("salary")*2;
        let salaryX3 = pm.request.body.formdata.get("salary")*3;
        pm.test ("Check salary[1] = salary from request * 2", function () {
            pm.expect(+jsonData.salary[1]).to.eql(salaryX2);
        });
        pm.test ("Check salary[2] = salary from request * 3", function () {
            pm.expect(+jsonData.salary[2]).to.eql(salaryX3);
        });

        // 4) проверить, что 2-й элемент массива salary больше 1-го и 0-го
        if (+jsonData.salary[2] > +jsonData.salary[1] && +jsonData.salary[2] > jsonData.salary[0]) {
            console.log("True");
        };
        pm.test("Check salaryX3 > salary", function () {
            pm.expect(+jsonData.salary[2]).to.be.above(jsonData.salary[0]);
        });
        pm.test("Check salaryX3 > salaryX2", function () {
            pm.expect(+jsonData.salary[2]).to.be.above(+jsonData.salary[1]);
        });
---

4) http://162.55.220.72:5005/test_pet_info
req. POST
age: int
weight: int
name: str
auth_token

Resp.
{'name': name,
 'age': age,
 'daily_food':weight * 0.012,
 'daily_sleep': weight * 2.5}

        // 1) Статус код 200
        pm.test("Status code is 200", function () {
            pm.response.to.have.status(200);
        });

        // 2) Проверка структуры json в ответе.
        let jsonData = pm.response.json();
        pm.test("Check schema", function () {
            pm.expect(jsonData).to.have.all.keys("age", "daily_food", "daily_sleep", "name");
        });

        // 3) В ответе указаны коэффициенты умножения weight, напишите тесты по проверке правильности результата перемножения на коэффициент.
        let reqData = pm.request.body.formdata;
        let reqWeight = pm.request.body.formdata.get("weight");
        let daily_food = reqWeight*0.012;
        let daily_sleep = reqWeight*2.5;
        pm.test ("Check daily_food", function () {
            pm.expect(jsonData.daily_food).to.eql(daily_food);
        });
        pm.test ("Check daily_sleep", function () {
            pm.expect(jsonData.daily_sleep).to.eql(daily_sleep);
        });
---

5) http://162.55.220.72:5005/get_test_user
req. POST
age: int
salary: int
name: str
auth_token

Resp.
{'name': name,
 'age':age,
 'salary': salary,
 'family':{'children':[['Alex', 24],['Kate', 12]],
 'u_salary_1.5_year': salary * 4}
  }

        // 1) Статус код 200
        pm.test("Status code is 200", function () {
            pm.response.to.have.status(200);
        });

        // 2) Проверка структуры json в ответе.
        let jsonData = pm.response.json();
        pm.test("Check schema", function () {
            pm.expect(jsonData).to.have.all.keys("age", "family", "name", "salary");
        });
        pm.test("Check schema2", function () {
            pm.expect(jsonData.family).to.have.all.keys("children", "u_salary_1_5_year");
        });

        // 3) Проверить, что значение поля name = значению переменной name из окружения
        let nameEnv = pm.environment.get("nameHW");
        pm.test ("Check name", function () {
            pm.expect(jsonData.name).to.eql(nameEnv);
        });

        // 4) Проверить, что значение поля age в ответе соответсвует отправленному в запросе значению поля age
        let reqData = pm.request.body.formdata;
        let reqAge = pm.request.body.formdata.get("age");
        pm.test ("Check age = age", function () {
            pm.expect(jsonData.age).to.eql(reqAge);
        });


