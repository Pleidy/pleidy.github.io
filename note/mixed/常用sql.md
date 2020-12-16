```sql
SELECT
	id,
	pallet_no,
	gateway_id,
	terminal_id,
	temper,
	accele_x,
	accele_y,
	accele_z,
	huoer_switch,
	warning_type,
	from_unixtime(gateway_time) AS 网关,
	from_unixtime(terminal_time) AS 终端,
	from_unixtime(created_at) AS 添加
FROM
	(
		SELECT DISTINCT
			*
		FROM
			pallet_convert_info
		WHERE
			gateway_id = '863412044172768'
		ORDER BY
			id DESC
	) c
GROUP BY
	terminal_id
ORDER BY
	id DESC;
```

