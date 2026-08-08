# 🔍 SQL Murder Mystery — Case Solution Report

**Case:** Murder in SQL City
**Date of Crime:** January 15, 2018
**Location:** SQL City

---

## 1. Locating the Crime Scene Report

The investigation begins by pulling the official crime report for the date and location in question.

```sql
SELECT * FROM crime_scene_report
WHERE date = 20180115 AND type = 'murder' AND city = 'SQL City';
```

The report reveals that **security footage caught two witnesses**:

| Witness | Location Clue |
|---|---|
| Witness #1 | Lives in the *last house* on **Northwestern Dr** |
| Witness #2 | Named **Annabel**, lives on **Franklin Ave** |

---

## 2. Identifying the Witnesses

### 2.1 Witness #1 — Last house on Northwestern Dr

```sql
SELECT * FROM person
WHERE address_street_name = 'Northwestern Dr'
ORDER BY address_number DESC LIMIT 1;
```

| id | name | address |
|---|---|---|
| 14887 | Morty Schapiro | 4919 Northwestern Dr |

### 2.2 Witness #2 — Annabel on Franklin Ave

```sql
SELECT * FROM person
WHERE address_street_name = 'Franklin Ave' AND name LIKE 'Annabel%';
```

| id | name | address |
|---|---|---|
| 16371 | Annabel Miller | 103 Franklin Ave |

---

## 3. Reading the Witness Interviews

```sql
SELECT * FROM interview
WHERE person_id = 14887 OR person_id = 16371;
```

> **Morty Schapiro (14887):**
> *"I heard a gunshot and then saw a man run out. He had a 'Get Fit Now Gym' bag. The membership number on the bag started with '48Z'. Only gold members have those bags. The man got into a car with a plate that included 'H42W'."*

> **Annabel Miller (16371):**
> *"I saw the murder happen, and I recognized the killer from my gym when I was working out last week on January the 9th."*

**Key clues gathered so far:**
- 🎒 Gym bag membership ID starting with `48Z`
- 🚗 License plate containing `H42W`
- 📅 Suspect worked out at the gym on **January 9th**

---

## 4. Narrowing Down Gym Members

```sql
SELECT * FROM get_fit_now_check_in
WHERE membership_id LIKE '48Z%';
```

No one checked in on the murder date itself, but the data confirms activity on **January 9th** — matching Annabel's statement.

```sql
SELECT * FROM get_fit_now_check_in
WHERE check_in_date = 20180109;
```

| membership_id | check_in_time | check_out_time |
|---|---|---|
| 48Z7A | 1600 | 1730 |
| 48Z55 | 1530 | 1700 |
| 90081 | 1600 | 1700 |

Cross-referencing membership IDs with member names:

```sql
SELECT * FROM get_fit_now_member
WHERE id = '48Z7A' OR id = '48Z55';
```

| id | person_id | name | status |
|---|---|---|---|
| 48Z7A | 28819 | Joe Germuska | gold |
| 48Z55 | 67318 | **Jeremy Bowers** | gold |

---

## 5. Matching the License Plate

```sql
SELECT * FROM drivers_license
WHERE plate_number LIKE '%H42W%';
```

| id | plate_number | car_make | car_model |
|---|---|---|---|
| 183779 | H42W0X | Toyota | Prius |
| 423327 | 0H42W2 | Chevrolet | Spark LS |
| 664760 | 4H42WR | Nissan | Altima |

Matching these license IDs to people:

```sql
SELECT * FROM person
WHERE license_id IN (183779, 423327, 664760);
```

| id | name | license_id |
|---|---|---|
| 51739 | Tushar Chandra | 664760 |
| **67318** | **Jeremy Bowers** | 423327 |
| 78193 | Maxine Whitely | 183779 |

🎯 **Jeremy Bowers matches every clue:** the gym bag membership, the January 9th check-in, and the license plate.

---

## 6. The Confession

```sql
SELECT * FROM interview WHERE person_id = 67318;
```

> **Jeremy Bowers (67318):**
> *"I was hired by a woman with a lot of money. I don't know her name but I know she's around 5'5" (65") or 5'7" (67"). She has red hair and she drives a Tesla Model S. I know that she attended the SQL Symphony Concert 3 times in December 2017."*

### ✅ Verdict: **Jeremy Bowers pulled the trigger** — but he was hired.

---

## 7. Bonus Round — Finding the Mastermind

Bowers' confession gives a new profile to search for: **red hair, female, Tesla Model S owner, ~5'5"–5'7", attended the SQL Symphony Concert 3x in Dec 2017.**

### 7.1 Filtering by physical description & car

```sql
SELECT * FROM drivers_license
WHERE hair_color = 'red' AND gender = 'female' AND car_make = 'Tesla';
```

| id | height | plate_number |
|---|---|---|
| 202298 | 66 | 500123 |
| 291182 | 66 | 08CM64 |
| 918773 | 65 | 917UU3 |

### 7.2 Matching to persons

```sql
SELECT * FROM person
WHERE license_id IN (202298, 291182, 918773);
```

| id | name |
|---|---|
| 78881 | Red Korb |
| 90700 | Regina George |
| 99716 | **Miranda Priestly** |

### 7.3 Verifying concert attendance

```sql
SELECT * FROM facebook_event_checkin
WHERE person_id IN (78881, 90700, 99716)
  AND event_name = 'SQL Symphony Concert'
  AND date BETWEEN 20171201 AND 20171231
ORDER BY person_id;
```

| person_id | date |
|---|---|
| 99716 | 2017-12-06 |
| 99716 | 2017-12-12 |
| 99716 | 2017-12-29 |

Only **Miranda Priestly** attended the concert exactly **3 times** in December 2017 — a perfect match.

### 7.4 Confirming her wealth

```sql
SELECT * FROM income WHERE ssn = 987756388;
```

| ssn | annual_income |
|---|---|
| 987756388 | $310,000 |

Ranking her against the full income table places her **47th out of 7,514** — comfortably in the top 1%, consistent with Bowers' description of "a woman with a lot of money."

---

## 🏁 Final Conclusion

| Role | Name |
|---|---|
| 🔫 Shooter | **Jeremy Bowers** |
| 💰 Mastermind who hired him | **Miranda Priestly** |

The case is solved: Jeremy Bowers committed the murder after being hired by Miranda Priestly, a wealthy woman matching every physical, vehicular, and behavioral clue uncovered in the investigation.
