# インターネットTVのテーブル設計
## テーブル：users
|カラム名|データ型|NULL|キー|初期値|AUTO<br>INCREMENT|
|:--:|:--:|:--:|:--:|:--:|:--:|
|user_id|BIGINT(20)||PRIMARY||Yes|
|user_name|VARCHAR(100)||INDEX||
|email|VARCHAR(100)||INDEX||
|passward_hash|VARCHAR(255)||||
|create_at|DATETIME|||||
* ユニークキー制約：user_nameカラム、emailカラムに対して設定
## テーブル：channels
|カラム名|データ型|NULL|キー|初期値|AUTO<br>INCREMENT|
|:--:|:--:|:--:|:--:|:--:|:--:|
|channel_id|INT(10)||PRIMARY||Yes|
|channel_name|VARCHAR(50)|||||
## テーブル：genres
|カラム名|データ型|NULL|キー|初期値|AUTO<br>INCREMENT|
|:--:|:--:|:--:|:--:|:--:|:--:|
|genre_id|INT(10)||PRIMARY||Yes|
|genre_name|VARCHAR(50)|||||
## テーブル：programs
|カラム名|データ型|NULL|キー|初期値|AUTO<br>INCREMENT|
|:--:|:--:|:--:|:--:|:--:|:--:|
|program_id|BIGINT(20)||PRIMARY||Yes|
|program_name|VARCHAR(50)|||||
|description|TEXT|||||
## テーブル：program_genres
|カラム名|データ型|NULL|キー|初期値|AUTO<br>INCREMENT|
|:--:|:--:|:--:|:--:|:--:|:--:|
|program_id|BIGINT(20)||INDEX|||
|genre_id|INT(10)||INDEX|||
* 外部キー制約：program_idに対してprogramsカラムのprogram_id、genre_idに対してgenresカラムのgenre_idから設定
* 複合インデックス：program_idとgenre_idに対して設定
## テーブル：episodes
|カラム名|データ型|NULL|キー|初期値|AUTO<br>INCREMENT|
|:--:|:--:|:--:|:--:|:--:|:--:|
|episode_id|BIGINT(20)||PRIMARY||Yes|
|program_id|BIGINT(20)||INDEX|||
|season_number|INT(10)|Yes||||
|episode_number|INT(10)|Yes||||
|title|VARCHAR(300)|||||
|description|TEXT|||||
|video_time|INT(10)|||||
|release_date|DATE|||||
* 外部キー制約：program_idに対してprogramsカラムのprogram_id、season_idに対してseasonsカラムのseason_idから設定
## テーブル：time_slot
|カラム名|データ型|NULL|キー|初期値|AUTO<br>INCREMENT|
|:--:|:--:|:--:|:--:|:--:|:--:|
|slot_id|BIGINT(20)||PRIMARY||Yes|
|channel_id|INT(10)||INDEX|||
|start_time|DATETIME|||||
|end_time|DATETIME|||||
* 外部キー制約：cahnnel_idに対してchannelsカラムのchannel_idから設定
## テーブル：broadcasts
|カラム名|データ型|NULL|キー|初期値|AUTO<br>INCREMENT|
|:--:|:--:|:--:|:--:|:--:|:--:|
|broadcast_id|BIGINT(20)||PRIMARY||Yes|
|slot_id|BIGINT(20)||INDEX|||
|episode_id|BIGINT(20)||INDEX|||
|view_count|INT(10)|||||
* 外部キー制約：slot_idに対してtime_slotカラムのslot_id、episode_idに対してepisodesカラムのepisode_idから設定
## テーブル：comments
|カラム名|データ型|NULL|キー|初期値|AUTO<br>INCREMENT|
|:--:|:--:|:--:|:--:|:--:|:--:|
|comment_id|BIGINT(20)||PRIMARY||Yes|
|user_id|BIGINT(20)||INDEX|||
|episode_id|BIGINT(20)||INDEX|||
|comment|TEXT|||||
|view_date|DATETIME|||||
* 外部キー制約：user_idに対してusersカラムのuser_id、episode_idに対してepisodesカラムのepisode_idから設定
## テーブル：view_history
|カラム名|データ型|NULL|キー|初期値|AUTO<br>INCREMENT|
|:--:|:--:|:--:|:--:|:--:|:--:|
|view_id|BIGINT(20)||PRIMARY||Yes|
|user_id|BIGINT(20)||INDEX|||
|episode_id|BIGINT(20)||INDEX|||
|view_date|DATETIME|||||
* 外部キー制約：user_idに対してusersカラムのuser_id、episode_idに対してepisodesカラムのepisode_idから設定
