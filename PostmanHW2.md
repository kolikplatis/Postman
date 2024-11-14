http://162.55.220.72:5005/first  
        
        //2. Статус код 200
        pm.test("Status code is 200", function () {
            pm.response.to.have.status(200);
        });

        //3. Проверить, что в body приходит правильный string.
        pm.test("Check string", function () {
            pm.response.to.have.body("This is the first responce from server!ss");
        });
---
http://162.55.220.72:5005/user_info_3    
        
        //2. Статус код 200
        pm.test("Status code is 200", function () {
            pm.response.to.have.status(200);
        });

        //3. Спарсить response body в json.
        let jsonData = pm.response.json();

        //4. Проверить, что name в ответе равно name s request (name вбить руками.)
        pm.test("Check name", function () {
            pm.expect(jsonData.name).to.eql("Oleg");
        });

        //5. Проверить, что age в ответе равно age s request (age вбить руками.)
        pm.test("Check age", function () {
            pm.expect(jsonData.age).to.eql("25");
        });

        //6. Проверить, что salary в ответе равно salary s request (salary вбить руками.)
        pm.test("Check salary", function () {
            pm.expect(jsonData.salary).to.eql(1160);
        });

        //7. Спарсить request.
        let reqData = pm.request.body.formdata;

        //8. Проверить, что name в ответе равно name s request (name забрать из request.)
        let reqName = pm.request.body.formdata.get("name");
        pm.test("Check name in the request and response", function () {
            pm.expect(jsonData.name).to.eql(reqName);
        });

        //9. Проверить, что age в ответе равно age s request (age забрать из request.)
        let reqAge = pm.request.body.formdata.get("age");
        pm.test("Check age in the request and response", function () {
            pm.expect(jsonData.age).to.eql(reqAge);
        });

        //10. Проверить, что salary в ответе равно salary s request (salary забрать из request.)
        let reqSalary = +pm.request.body.formdata.get("salary");
        pm.globals.set("variable_key", "variable_value");
        pm.test("Check salary in the request and response", function () {
            pm.expect(jsonData.salary).to.eql(reqSalary);
        });

        //11. Вывести в консоль параметр family из response.
        console.log(jsonData.family)

        //12. Проверить что u_salary_1_5_year в ответе равно salary*4 (salary забрать из request)
        let reqSalary_4 = pm.request.body.formdata.get("salary")*4;
        let respSalary_1_5 = jsonData.family.u_salary_1_5_year;
        pm.test("Check u_salary_1_5_year = salary * 4", function () {
            pm.expect(respSalary_1_5).to.eql(reqSalary_4);
        });
---

http://162.55.220.72:5005/object_info_3  
        
        //2. Статус код 200
        pm.test("Status code is 200", function () {
            pm.response.to.have.status(200);
        });

        //3. Спарсить response body в json.
        let jsonData = pm.response.json();

        //4. Спарсить request.
        let reqData = pm.request.url.query.toObject();

        // 5. Проверить, что name в ответе равно name из request (name забрать из request.)
        pm.test("Check name", function () {
            pm.expect(jsonData.name).to.eql(reqData.name);
        });

        // 6. Проверить, что age в ответе равно age из request (age забрать из request.)
        pm.test("Check age", function () {
            pm.expect(jsonData.age).to.eql(reqData.age);
        });

        // 7. Проверить, что salary в ответе равно salary из request (salary забрать из request.)
        pm.test("Check salary", function () {
            pm.expect(jsonData.salary).to.eql(+reqData.salary);
        });

        // 8. Вывести в консоль параметр family из response.
        console.log(jsonData.family);

        // 9. Проверить, что у параметра dog есть параметры name.
        let keyDog = jsonData.family.pets.dog;
        pm.test("Dog has a name", function (){
            pm.expect(keyDog).to.have.property("name");
        });

        // 10. Проверить, что у параметра dog есть параметры age.
        pm.test("Dog has a age", function (){
            pm.expect(keyDog).to.have.property("age");
        });

        // 11. Проверить, что параметр name имеет значение Luky.
        let dogName = jsonData.family.pets.dog.name;
        pm.test("Name is Luky", function (){
            pm.expect(dogName).to.include("Luky");
        });

        // 12. Проверить, что параметр age имеет значение 4.
        let dogAge = jsonData.family.pets.dog.age;
        pm.test("Age = 4", function (){
            pm.expect(dogAge).to.eql(4);
        });
---

http://162.55.220.72:5005/object_info_4  
        
        // 2. Статус код 200
        pm.test("Status code is 200", function () {
            pm.response.to.have.status(200);
        });

        // 3. Спарсить response body в json.
        let jsonData = pm.response.json();

        // 4. Спарсить request.
        let reqData = pm.request.url.query.toObject();

        // 5. Проверить, что name в ответе равно name из request (name забрать из request.)
        pm.test("Check name", function () {
            pm.expect(jsonData.name).to.eql(reqData.name);
        });

        // 6. Проверить, что age в ответе равно age из request (age забрать из request.)
        let reqAge = +reqData.age
        pm.test("Check age", function () {
            pm.expect(jsonData.age).to.eql(reqAge);
        });

        // 7. Вывести в консоль параметр salary из request.
        console.log(reqData.salary);

        // 8. Вывести в консоль параметр salary из response.
        console.log(jsonData.salary);

        // 9. Вывести в консоль 0-й элемент параметра salary из response.
        console.log(jsonData.salary[0]);

        // 10. Вывести в консоль 1-й элемент параметра salary из response.
        console.log(jsonData.salary[1]);

        // 11. Вывести в консоль 2-й элемент параметра salary из response.
        console.log(jsonData.salary[2]);

        // 12. Проверить, что 0-й элемент параметра salary равен salary из request (salary забрать из request.)
        let salary0 = jsonData.salary[0];
        let reqSalary = +reqData.salary;
        pm.test ("Check salary 0", function () {
            pm.expect(salary0).to.eql(reqSalary);
        });

        // 13. Проверить, что 1-й элемент параметра salary равен salary*2 из request (salary забрать из request.)
        let salary1 = +jsonData.salary[1];
        let reqSalaryX2 = reqData.salary*2
        pm.test("Check salary[1] = salary * 2", function () {
            pm.expect(salary1).to.eql(reqSalaryX2);
        });

        // 14. Проверить, что 2-й элемент параметра salary равен salary*3 из request (salary забрать из request.)
        let salary2 = +jsonData.salary[2];
        let reqSalaryX3 = reqData.salary*3
        pm.test("Check salary[2] = salary * 3", function () {
            pm.expect(salary2).to.eql(reqSalaryX3);
        });

        // 15. Создать в окружении переменную name
        pm.environment.set("nameHW");

        // 16. Создать в окружении переменную age
        pm.environment.set("ageHW");

        // 17. Создать в окружении переменную salary
        pm.environment.set("salaryHW");

        // 18. Передать в окружение переменную name
        pm.environment.set("nameHW", jsonData.name);

        // 19. Передать в окружение переменную age
        pm.environment.set("ageHW", jsonData.age);

        // 20. Передать в окружение переменную salary
        pm.environment.set("salaryHW", jsonData.salary[0]);

        // 21. Написать цикл который выведет в консоль по порядку элементы списка из параметра salary.
        for (let i = 0; i<3; i++) {
            console.log(jsonData.salary[i]);
        };
---
http://162.55.220.72:5005/user_info_2  
        
        // 5. Статус код 200
        pm.test("Status code is 200", function () {
            pm.response.to.have.status(200);
        });

        // 6. Спарсить response body в json.
        let jsonData = pm.response.json();

        // 7. Спарсить request.
        let reqData = pm.request.body.formdata;

        // 8. Проверить, что json response имеет параметр start_qa_salary
        pm.test("Check start_qa_salary", function (){
            pm.expect(jsonData).to.have.property("start_qa_salary");
        });

        // 9. Проверить, что json response имеет параметр qa_salary_after_6_months
        pm.test("Check qa_salary_after_6_months", function (){
            pm.expect(jsonData).to.have.property("qa_salary_after_6_months");
        });

        // 10. Проверить, что json response имеет параметр qa_salary_after_12_months
        pm.test("Check qa_salary_after_12_months", function (){
            pm.expect(jsonData).to.have.property("qa_salary_after_12_months");
        });

        // 11. Проверить, что json response имеет параметр qa_salary_after_1.5_year
        pm.test("Check qa_salary_after_1.5_year", function (){
            pm.expect(jsonData).to.have.property("qa_salary_after_1.5_year");
        });

        // 12. Проверить, что json response имеет параметр qa_salary_after_3.5_years
        pm.test("Check qa_salary_after_3.5_years", function (){
            pm.expect(jsonData).to.have.property("qa_salary_after_3.5_years");
        });

        // 13. Проверить, что json response имеет параметр person
        pm.test("Check person", function (){
            pm.expect(jsonData).to.have.property("person");
        });

        // 14. Проверить, что параметр start_qa_salary равен salary из request (salary забрать из request.)
        let reqSalary = +pm.request.body.formdata.get("salary");
        pm.test("start_qa_salary = reqSalary", function () {
            pm.expect(jsonData.start_qa_salary).to.eql(reqSalary);
        });

        // 15. Проверить, что параметр qa_salary_after_6_months равен salary*2 из request (salary забрать из request.)
        let reqSalaryX2 = pm.request.body.formdata.get("salary")*2;
        pm.test("qa_salary_after_6_months = reqSalary*2", function () {
            pm.expect(jsonData.qa_salary_after_6_months).to.eql(reqSalaryX2);
        });

        // 16. Проверить, что параметр qa_salary_after_12_months равен salary*2.7 из request (salary забрать из request.)
        let reqSalaryX2_7 = pm.request.body.formdata.get("salary")*2.7;
        pm.test("qa_salary_after_12_months = reqSalary*2.7", function () {
            pm.expect(jsonData.qa_salary_after_12_months).to.eql(reqSalaryX2_7);
        });

        // 17. Проверить, что параметр qa_salary_after_1.5_year равен salary*3.3 из request (salary забрать из request.)
        let reqSalaryX3_3 = pm.request.body.formdata.get("salary")*3.3;
        pm.test("qa_salary_after_1.5_year = reqSalary*3.3", function () {
            pm.expect(jsonData["qa_salary_after_1.5_year"]).to.eql(reqSalaryX3_3);
        });

        // // 18. Проверить, что параметр qa_salary_after_3.5_years равен salary*3.8 из request (salary забрать из request.)
        let reqSalaryX3_8 = pm.request.body.formdata.get("salary")*3.8;
        pm.test("qa_salary_after_3.5_years = reqSalary*3.8", function () {
            pm.expect(jsonData["qa_salary_after_3.5_years"]).to.eql(reqSalaryX3_8);
        });

        // 19. Проверить, что в параметре person, 1-й элемент из u_name равен salary из request (salary забрать из request.)
        let respName = jsonData.person.u_name[1];
        pm.test("Check person.u_name[1] = name from request", function() {
            pm.expect(respName).to.eql(reqSalary);
        });
        // 20. Проверить, что что параметр u_age равен age из request (age забрать из request.)
        let respAge = jsonData.person.u_age;
        let reqAge = +pm.request.body.formdata.get("age");
        pm.test("Check person.u_age = age from request", function() {
            pm.expect(respAge).to.eql(reqAge);
        });

        // 21. Проверить, что параметр u_salary_5_years равен salary*4.2 из request (salary забрать из request.)
        let reqSalaryX4_2 = pm.request.body.formdata.get("salary")*4.2;
        let salary5Years = jsonData.person.u_salary_5_years;
        pm.test("u_salary_5_years = reqSalary*2.7", function () {
            pm.expect(salary5Years).to.eql(reqSalaryX4_2);
        });

        // 22. ***Написать цикл который выведет в консоль по порядку элементы списка из параметра person
        let jsonPerson = jsonData.person.u_name;
        for (let i = 0; i<jsonPerson.length; i++) {
            console.log(jsonPerson[i]);
        };