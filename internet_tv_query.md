# 特定のデータを抽出するクエリ
1. よく見られているエピソードを知りたいです。エピソード視聴数トップ3のエピソードタイトルと視聴数を取得してください
```mysql:MySQL
SELECT title, sum(view_count) FROM broadcasts
INNER JOIN episodes 
ON broadcasts.episode_id = episodes.episode_id 
GROUP BY broadcasts.episode_id 
ORDER BY sum(view_count) DESC LIMIT 3;
```
2. よく見られているエピソードの番組情報やシーズン情報も合わせて知りたいです。エピソード視聴数トップ3の番組タイトル、シーズン数、エピソード数、エピソードタイトル、視聴数を取得してください
```mysql:MySQL
SELECT program_name, season_number, episode_number, title, sum(view_count) FROM episodes 
INNER JOIN broadcasts 
ON broadcasts.episode_id = episodes.episode_id 
JOIN programs 
ON episodes.program_id = programs.program_id 
GROUP BY broadcasts.episode_id 
ORDER BY sum(view_count) DESC LIMIT 3;
```
3. 本日の番組表を表示するために、本日、どのチャンネルの、何時から、何の番組が放送されるのかを知りたいです。本日放送される全ての番組に対して、チャンネル名、放送開始時刻(日付+時間)、放送終了時刻、シーズン数、エピソード数、エピソードタイトル、エピソード詳細を取得してください。なお、番組の開始時刻が本日のものを本日方法される番組とみなすものとします
```mysql:MySQL
SELECT channel_name, start_time, end_time, season_number, episode_number, title, description FROM broadcasts AS b 
INNER JOIN episodes AS e ON b.episode_id = e.episode_id 
INNER JOIN time_slot AS t ON b.slot_id = t.slot_id 
INNER JOIN channels AS c ON t.channel_id = c.channel_id 
WHERE start_time < '2024-07-02 00:00:00';
```
4. ドラマというチャンネルがあったとして、ドラマのチャンネルの番組表を表示するために、本日から一週間分、何日の何時から何の番組が放送されるのかを知りたいです。ドラマのチャンネルに対して、放送開始時刻、放送終了時刻、シーズン数、エピソード数、エピソードタイトル、エピソード詳細を本日から一週間分取得してください
```mysql:MySQL
SELECT start_time, end_time, season_number, episode_number, title, description FROM broadcasts AS b 
INNER JOIN episodes AS e ON b.episode_id = e.episode_id 
INNER JOIN time_slot AS t ON b.slot_id = t.slot_id 
WHERE start_time >= '2024-07-01 00:00:00' AND start_time < '2024-07-08 00:00:00';
```
5. 直近一週間で最も見られた番組が知りたいです。直近一週間に放送された番組の中で、エピソード視聴数合計トップ2の番組に対して、番組タイトル、視聴数を取得してください
```mysql:MySQL
SELECT program_name, sum(view_count) FROM broadcasts AS b 
INNER JOIN episodes AS e ON b.episode_id = e.episode_id 
INNER JOIN time_slot AS t ON b.slot_id = t.slot_id 
INNER JOIN programs AS p ON e.program_id = p.program_id 
WHERE start_time >= '2024-07-01 00:00:00' AND start_time < '2024-07-8 00:00:00' 
GROUP BY e.program_id 
ORDER BY sum(view_count) DESC LIMIT 2;
```
6. ジャンルごとの番組の視聴数ランキングを知りたいです。番組の視聴数ランキングはエピソードの平均視聴数ランキングとします。ジャンルごとに視聴数トップの番組に対して、ジャンル名、番組タイトル、エピソード平均視聴数を取得してください。
```mysql:MySQL
WITH top_count AS (
    SELECT genre_name, program_name, AVG(view_count) AS avg_view_count, 
    ROW_NUMBER() OVER(PARTITION BY g.genre_id ORDER BY AVG(view_count) DESC) AS count_rank 
    FROM broadcasts AS b 
    INNER JOIN episodes AS e ON b.episode_id = e.episode_id 
    INNER JOIN programs AS p ON e.program_id = p.program_id 
    INNER JOIN program_genres AS pg ON p.program_id = pg.program_id 
    INNER JOIN genres AS g ON pg.genre_id = g.genre_id 
    GROUP BY p.program_id, g.genre_id
) 
SELECT genre_name, program_name, avg_view_count 
FROM top_count 
WHERE count_rank = 1;
```