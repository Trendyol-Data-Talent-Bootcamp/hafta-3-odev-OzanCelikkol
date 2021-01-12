# Soru 1

```SQL
with solution as
(
select name, car, manufacture.year as manufacture_year, array(select as struct p.* replace(timestamp_add(p.date, interval 3 hour) as date) from unnest(purchase) as p) as purchase_modified
from ozan_celikkol.semi_structed_hw as hw
cross join unnest(manufacture) as manufacture
cross join unnest(car) as car
on manufacture.id = car.id
),

/* Below from here is for check. */

hash_table_1 as
(
select *, farm_fingerprint(to_json_string(original_target)) as hash_check_1
from ozan_celikkol.semi_structed_hw_expected as original_target
),

hash_table_2 as
(
select *, farm_fingerprint(to_json_string(created_target)) as hash_check_2
from solution as created_target
)

select *
from hash_table_1
right outer join hash_table_2
on hash_table_1.hash_check_1 = hash_table_2.hash_check_2
where hash_table_1.hash_check_1 <> hash_table_2.hash_check_2
```
