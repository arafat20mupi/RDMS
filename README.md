# PostgreSQL সম্পূর্ণ গাইড (সহজ ও বিস্তারিত)

---

## PostgreSQL কি?

PostgreSQL হলো একটি ওপেন সোর্স রিলেশনাল ডাটাবেজ ম্যানেজমেন্ট সিস্টেম (RDBMS)।
ডাটা সেভ, ম্যানেজ এবং দ্রুত অনুসন্ধানের জন্য ব্যবহৃত হয়।

---

## ১. ডাটাবেজে টেবিল তৈরি ও ডাটা ম্যানিপুলেশন

### CREATE TABLE

নতুন টেবিল তৈরি করার জন্য

```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,       -- ইউনিক আইডি, অটোমেটিক বাড়বে
    name VARCHAR(100),           -- নাম (সর্বোচ্চ ১০০ ক্যারেক্টার)
    age INT                      -- বয়স (পূর্ণসংখ্যা)
);
```

**বাংলা অর্থ:** `users` নামের একটি টেবিল তৈরি করলাম যার মধ্যে id, name, ও age আছে।

---

### INSERT INTO

টেবিলে ডাটা ঢোকানোর জন্য

```sql
INSERT INTO users (name, age) VALUES ('Arafat', 25);
```

**বাংলা অর্থ:** `users` টেবিলে ‘Arafat’ নামের ২৫ বছর বয়সী ইউজারের ডাটা ঢোকানো।

---

### SELECT

টেবিল থেকে ডাটা বের করার জন্য

```sql
SELECT * FROM users;
SELECT name, age FROM users WHERE age > 20;
```

**বাংলা অর্থ:** সব ইউজারের তথ্য দেখাও অথবা ২০ বছরের বেশি বয়সের ইউজারদের দেখাও।

---

### UPDATE

বিদ্যমান ডাটা পরিবর্তনের জন্য

```sql
UPDATE users SET age = 26 WHERE name = 'Arafat';
```

**বাংলা অর্থ:** যাদের নাম ‘Arafat’ তাদের বয়স ২৬ সেট করো।

---

### DELETE

ডাটা মুছে ফেলার জন্য

```sql
DELETE FROM users WHERE id = 1;
```

**বাংলা অর্থ:** যাদের id=১ তাদের ডাটা মুছে ফেলো।

---

## ২. টেবিল কাঠামো পরিবর্তন

### ALTER TABLE

টেবিলের কাঠামো পরিবর্তনের জন্য যেমন নতুন কলাম যোগ করা, নাম পরিবর্তন ইত্যাদি

```sql
ALTER TABLE users ADD COLUMN email VARCHAR(100);
ALTER TABLE users DROP COLUMN email;
```

**বাংলা অর্থ:** টেবিলে নতুন `email` কলাম যোগ বা মুছে ফেলো।

---

## ৩. কী (Key) ও টেবিলের সম্পর্ক

### PRIMARY KEY

একটি ইউনিক আইডেন্টিফায়ার যেটা প্রতিটি রেকর্ড আলাদা করে।

```sql
id SERIAL PRIMARY KEY
```

**বাংলা:** `id` কলামটি প্রতিটি রেকর্ডের জন্য ইউনিক।

---

### FOREIGN KEY

এক টেবিলের কলাম যা অন্য টেবিলের PRIMARY KEY কে রেফার করে। এটি দুই টেবিলের সম্পর্ক তৈরি করে।

```sql
CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    user_id INT REFERENCES users(id), -- users টেবিলের id কে রেফার করে
    total NUMERIC
);
```

**বাংলা:** `orders` টেবিলের `user_id` কলাম `users` টেবিলের `id` এর সাথে যুক্ত।

---

## ৪. ডাটা অনুসন্ধান ও ফিল্টারিং

### WHERE

শর্ত দিয়ে ডাটা ফিল্টার করা হয়

```sql
SELECT * FROM users WHERE age > 18;
```

**বাংলা:** ১৮ বছরের বেশি বয়সের ইউজার দেখাও।

---

### LOGICAL OPERATORS

AND, OR, NOT দিয়ে শর্ত তৈরি করা যায়

```sql
SELECT * FROM users WHERE age > 18 AND name LIKE 'A%';
```

**বাংলা:** বয়স ১৮ এর বেশি এবং নাম ‘A’ দিয়ে শুরু এমন ইউজার দেখাও।

---

### GROUP BY

একই ধরনের ডাটা গুলোকে গ্রুপ করে ফেলা

```sql
SELECT age, COUNT(*) FROM users GROUP BY age;
```

**বাংলা:** বয়স অনুযায়ী গ্রুপ করে প্রতিটি বয়সে কতজন আছে দেখাও।

---

### HAVING

GROUP BY এর পর গ্রুপগুলোর মধ্যে শর্ত আরোপ করা

```sql
SELECT age, COUNT(*) FROM users GROUP BY age HAVING COUNT(*) > 1;
```

**বাংলা:** যেসব বয়সের গোষ্ঠীতে ১ এর বেশি সদস্য আছে তাদের দেখাও।

---

### ORDER BY

ডাটা সাজানোর জন্য

```sql
SELECT * FROM users ORDER BY age DESC;
```

**বাংলা:** বয়সের ভিত্তিতে নামানোর ক্রমে দেখাও।

---

## ৫. JOIN - টেবিল একত্রিত করা

### INNER JOIN

যেখানে দুই টেবিলের মিল থাকে সেই ডাটা বের করে

```sql
SELECT u.name, o.total 
FROM users u 
INNER JOIN orders o ON u.id = o.user_id;
```

**বাংলা:** ইউজারদের নাম এবং তাদের অর্ডারের টোটাল দেখাও যেখানে সম্পর্ক আছে।

---

### LEFT JOIN

বাম পাশের টেবিলের সব ডাটা এবং ডান পাশের টেবিলের মিল থাকা ডাটা দেখায়।

```sql
SELECT u.name, o.total 
FROM users u 
LEFT JOIN orders o ON u.id = o.user_id;
```

**বাংলা:** সব ইউজার দেখাও, যারা অর্ডার দিয়েছে তাদের অর্ডার টোটালসহ।

---

### RIGHT JOIN

ডান পাশের টেবিলের সব ডাটা এবং বাম পাশের টেবিলের মিল থাকা ডাটা দেখায়।

---

### FULL JOIN

দুই টেবিলের সব ডাটা মিলিয়ে দেখায়।

---

## ৬. INDEX - দ্রুত অনুসন্ধানের জন্য

```sql
CREATE INDEX idx_name ON users(name);
```

**বাংলা:** `users` টেবিলের `name` কলামের জন্য দ্রুত অনুসন্ধানের ইনডেক্স তৈরি করো।

---

## ৭. TRANSACTION - একাধিক কাজ একসাথে

```sql
BEGIN;
UPDATE accounts SET balance=balance-100 WHERE id=1;
UPDATE accounts SET balance=balance+100 WHERE id=2;
COMMIT;
```

**বাংলা:** টাকা একাউন্ট ১ থেকে ২ তে ট্রান্সফার করো, সব ঠিক থাকলে সেভ করো।

---

## ৮. VIEW - ভার্চুয়াল টেবিল

```sql
CREATE VIEW user_orders AS
SELECT u.name, o.total
FROM users u
JOIN orders o ON u.id = o.user_id;
```

**বাংলা:** ইউজার ও তাদের অর্ডারের তথ্য দেখানোর জন্য একটি ভার্চুয়াল টেবিল তৈরি করো।

---

## ৯. FUNCTION & TRIGGER

### FUNCTION

SQL কোডের রিইউজেবল ব্লক

```sql
CREATE FUNCTION get_discount(price NUMERIC) RETURNS NUMERIC AS $$
BEGIN
  RETURN price * 0.9;
END;
$$ LANGUAGE plpgsql;
```

**বাংলা:** ১০% ডিসকাউন্ট হিসাব করার ফাংশন।

---

### TRIGGER

স্বয়ংক্রিয় কাজের জন্য ইভেন্ট ভিত্তিক ফাংশন

```sql
CREATE TRIGGER log_update
AFTER UPDATE ON users
FOR EACH ROW
EXECUTE FUNCTION log_change();
```

**বাংলা:** `users` টেবিল আপডেট হলে `log_change` ফাংশন চালাও।

---

## ১০. USER & PERMISSIONS

### GRANT

অনুমতি দেওয়া

```sql
GRANT SELECT ON users TO readonly_user;
```

**বাংলা:** `readonly_user` কে `users` টেবিল পড়ার অনুমতি দাও।

---

### REVOKE

অনুমতি প্রত্যাহার

```sql
REVOKE INSERT ON users FROM readonly_user;
```

**বাংলা:** `readonly_user` এর লেখার অনুমতি তুলে নাও।

---

## ১১. BACKUP & RESTORE

```bash
pg_dump mydb > backup.sql      # ব্যাকআপ নেওয়া
psql mydb < backup.sql         # ব্যাকআপ থেকে রিস্টোর করা
```

**বাংলা:** ডাটাবেজের ব্যাকআপ নেয়া এবং পুনরায় রিস্টোর করা।

---

## ১২. ডাটা টাইপস

| টাইপ       | ব্যবহার                 | বাংলা অর্থ             |
| ---------- | ----------------------- | ---------------------- |
| SERIAL     | স্বয়ংক্রিয় বৃদ্ধি পায় | অটোমেটিক ইউনিক সংখ্যা  |
| INT        | পূর্ণসংখ্যা             | পূর্ণ সংখ্যা           |
| VARCHAR(n) | চরিত্রের স্ট্রিং        | সীমিত দৈর্ঘ্যের টেক্সট |
| TEXT       | বড় টেক্সট               | বড় টেক্সট ডাটা        |
| NUMERIC    | দশমিক সংখ্যা            | দশমিক সহ নম্বর         |
| BOOLEAN    | সত্য/মিথ্যা             | true/false মান         |
| DATE       | তারিখ                   | তারিখের তথ্য           |
| TIMESTAMP  | সময় ও তারিখ             | সময়সহ তারিখ            |

---

## ১৩. পিএসকিউএল (psql) কমান্ড

```bash
psql -U username -d dbname
\l           -- ডাটাবেজ লিস্ট দেখাও
\c dbname    -- একটি ডাটাবেজে কানেক্ট করো
\dt          -- টেবিলের তালিকা দেখাও
\d tablename -- টেবিলের কাঠামো দেখাও
```
