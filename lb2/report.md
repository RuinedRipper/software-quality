# Лабораторна робота №2. Проєктування тестів. Checklist, Test Cases та Decision Table

**Виконав:** Ушаков Ростислав
**Група:** ПІ:6.1214-пі-с2
**Дата виконання:** 17.11.2026

## Test Object

SauceDemo — Login: https://www.saucedemo.com/

## Test Conditions

- TCND-01 — успішна авторизація валідного користувача (TB-03, TB-04);
- TCND-02 — авторизація з неправильним Username (TB-06);
- TCND-03 — авторизація з неправильним Password (TB-06);
- TCND-04 — авторизація з порожнім Username (TB-01);
- TCND-05 — авторизація з порожнім Password (TB-02);
- TCND-06 — авторизація заблокованого користувача (TB-05).

## Checklist

- [ ] Успішна авторизація з валідними даними.
- [ ] Відмова в авторизації з неправильним Username.
- [ ] Відмова в авторизації з неправильним Password.
- [ ] Перевірка порожнього Username.
- [ ] Перевірка порожнього Password.
- [ ] Відмова в авторизації заблокованого користувача.

## Test Cases

### TC-LOGIN-01 — Успішна авторизація standard_user

**Type:** Positive

**Preconditions:**
- відкрита сторінка Login SauceDemo;
- користувач не авторизований.

**Test Data:**
- Username: standard_user
- Password: secret_sauce

**Steps:**
1. У поле Username ввести standard_user.
2. У поле Password ввести secret_sauce.
3. Натиснути кнопку Login.

**Expected Result:**
Після введення standard_user і правильного пароля та натискання Login користувач успішно авторизується і переходить на сторінку Products.

**Actual Result:**
Після введення standard_user і правильного пароля та натискання Login відкрилася сторінка Products (/inventory.html).

**Result:** Pass

### TC-LOGIN-02 — Відмова в авторизації з правильним Username та неправильним Password

**Type:** Negative

**Preconditions:**
- відкрита сторінка Login SauceDemo;
- користувач не авторизований.

**Test Data:**
- Username: standard_user
- Password: wrong_password

**Steps:**
1. У поле Username ввести standard_user.
2. У поле Password ввести wrong_password.
3. Натиснути кнопку Login.

**Expected Result:**
Після введення standard_user і неправильного пароля та натискання Login авторизація не виконується, користувач залишається на сторінці Login, відображається повідомлення "Epic sadface: Username and password do not match any user in this service".

**Actual Result:**
Після введення standard_user і wrong_password та натискання Login користувач залишився на сторінці Login, відобразилося повідомлення "Epic sadface: Username and password do not match any user in this service".

**Result:** Pass

### TC-LOGIN-03 — Відмова в авторизації заблокованого користувача locked_out_user

**Type:** Negative

**Preconditions:**
- відкрита сторінка Login SauceDemo;
- користувач не авторизований.

**Test Data:**
- Username: locked_out_user
- Password: secret_sauce

**Steps:**
1. У поле Username ввести locked_out_user.
2. У поле Password ввести secret_sauce.
3. Натиснути кнопку Login.

**Expected Result:**
Після введення locked_out_user і правильного пароля та натискання Login авторизація не виконується, користувач залишається на сторінці Login, відображається повідомлення "Epic sadface: Sorry, this user has been locked out."

**Actual Result:**
Після введення locked_out_user і secret_sauce та натискання Login користувач залишився на сторінці Login, відобразилося повідомлення "Epic sadface: Sorry, this user has been locked out."

**Result:** Pass

## Decision Table

**Умови:**
- C1 — Username входить до списку допустимих?
- C2 — Password правильний?
- C3 — Користувач заблокований?

**Дії:**
- A1 — перейти до Products;
- A2 — показати повідомлення про блокування;
- A3 — показати повідомлення про неправильні credentials.

| Умови / Дії | R1 | R2 | R3 | R4 | R5 |
|---|---|---|---|---|---|
| C1: Username допустимий? | T | T | F | T | F |
| C2: Password правильний? | T | T | T | F | F |
| C3: Користувач заблокований? | F | T | - | - | - |
| A1: Перехід до Products | X | | | | |
| A2: Повідомлення про блокування | | X | | | |
| A3: Повідомлення про невалідні credentials | | | X | X | X |

Розшифровка правил:
- R1: valid Username + valid Password + не заблокований → Products.
- R2: valid Username + valid Password + заблокований → відмова (повідомлення про блокування).
- R3: invalid Username + valid Password → відмова (невалідні credentials).
- R4: valid Username + invalid Password → відмова (невалідні credentials).
- R5: invalid Username + invalid Password → відмова (невалідні credentials).

Примітка: випадки «порожній Username» та «порожній Password» не включені до таблиці, оскільки SauceDemo перевіряє їх окремо до перевірки credentials; вони покриті TCND-04 і TCND-05 у checklist.

### Відповідність правил та Test Cases

| Test Case | Правило |
|---|---|
| TC-LOGIN-01 | R1 |
| TC-LOGIN-02 | R4 |
| TC-LOGIN-03 | R2 |

**Правила без окремого Test Case:** R3 (invalid Username + valid Password) та R5 (invalid Username + invalid Password). Decision Table показала, що перевірка неіснуючого Username (TCND-02) не має власного тест-кейсу.

## Результати виконання

| Test Case | Type | Result |
|---|---|---|
| TC-LOGIN-01 | Positive | Pass |
| TC-LOGIN-02 | Negative | Pass |
| TC-LOGIN-03 | Negative | Pass |

## Висновок

Було визначено 6 Test Conditions на основі Test Basis, сформовано checklist, описано 3 Test Cases (1 позитивний, 2 негативні) та побудовано Decision Table для комбінацій облікових даних. Усі три Test Cases виконано в SauceDemo, фактичні результати збіглися з очікуваними (Pass). Decision Table виявила, що правила R3 і R5 не покриті окремими Test Cases, тому їх варто додати при розширенні тестової документації.

## Відповіді на контрольні питання

1. **Що таке Test Basis і навіщо він потрібний?**
   Test Basis — вихідна документація або вимоги, на основі яких проєктуються тести. Він потрібен як еталон для визначення Expected Result, щоб не покладатися на власні припущення.

2. **Чим Test Condition відрізняється від Test Case?**
   Test Condition — стисла ідея того, що потрібно перевірити. Test Case — детальна інструкція перевірки: передумови, дані, кроки та очікуваний результат.

3. **На яке питання відповідає Test Analysis?**
   «Що саме потрібно перевірити?» — результатом є Test Conditions.

4. **На яке питання відповідає Test Design?**
   «Як саме це перевірити?» — створення Test Cases, добір даних і кроків.

5. **Для чого використовується checklist?**
   Це компактний спосіб зафіксувати, що потрібно перевірити, без детального опису дій; зручний для швидкої, регресійної чи дослідницької перевірки.

6. **Чому checklist не завжди може замінити детальний test case?**
   У ньому немає конкретних даних, кроків і чіткого Expected Result, тому різні тестувальники можуть виконати та інтерпретувати пункт по-різному.

7. **Чим positive test відрізняється від negative test?**
   Positive перевіряє штатну поведінку з коректними даними; negative перевіряє реакцію системи на некоректні дані чи дії (обробку помилок).

8. **Чому negative test може мати результат Pass?**
   Pass означає, що фактична поведінка збіглася з очікуваною. Якщо за вимогами система має відхилити некоректні дані та показати помилку, і вона це робить — тест пройдено.

9. **Для чого використовується Decision Table?**
   Для систематизації комбінацій умов і відповідних дій системи, щоб не пропустити важливі комбінації.

10. **Навіщо порівнювати Expected Result та Actual Result?**
    Щоб об'єктивно визначити, чи відповідає поведінка системи вимогам: розбіжність вказує на можливий дефект.

11. **Що означають Pass та Fail?**
    Pass — Actual Result збігається з Expected Result. Fail — є будь-яка розбіжність між ними.