---
title: ARASHI Songs
---

<Details title='What is this?'>

  This is a dashboard about ARASHI songs, such as song play counts and DVD/Blu-ray recordings.
</Details>

```sql songs_summary
  select
      COUNT(曲名) as song_count
  from arashi_songs.arashi_songs
```

```sql songs_type_summary
  select
      COUNT(曲名) as song_count,
      タイプ as release_type
  from arashi_songs.arashi_songs
  group by release_type
```

```sql play_count_summary
  select
      SUM(再生回数) as play_count
  from arashi_songs.arashi_songs
```

```sql play_count_type_summary
  select
      SUM(再生回数) as song_count,
      タイプ as release_type
  from arashi_songs.arashi_songs
  group by release_type
```

```sql rap_songs_summary
  select
      COUNT(曲名) as song_count
  from arashi_songs.arashi_songs
  where
    クレジット like '%SHOW%' or 
    クレジット like '%櫻井翔%' or 
    クレジット like '%Sho Sakurai%'
```

```sql rap_songs_type_summary
  select
      COUNT(曲名) as song_count,
      タイプ as release_type
  from arashi_songs.arashi_songs
  where
    クレジット like '%SHOW%' or 
    クレジット like '%櫻井翔%' or 
    クレジット like '%Sho Sakurai%'
  group by release_type
```

## Summary

<BigValue
  data={songs_summary}
  value=song_count
  title="Songs"
/>

Single: <Value data={songs_type_summary} column=song_count row=0 />, c/w: <Value data={songs_type_summary} column=song_count row=1 />, Album: <Value data={songs_type_summary} column=song_count row=2 />

<BigValue
  data={rap_songs_summary}
  value=song_count
  title="Sakurap Songs"
/>

Single: <Value data={rap_songs_type_summary} column=song_count row=0 />, c/w: <Value data={rap_songs_type_summary} column=song_count row=1 />, Album: <Value data={rap_songs_type_summary} column=song_count row=2 />

<BigValue
  data={play_count_summary}
  value=play_count
  title="Play Count"
  fmt=num0
/>

Single: <Value data={play_count_type_summary} column=song_count row=2 fmt=num0 />, c/w: <Value data={play_count_type_summary} column=song_count row=1 fmt=num0 />, Album: <Value data={play_count_type_summary} column=song_count row=0 fmt=num0 />

## Sing in Concert Top 20

<Dropdown 
  name=release_type_concert
  title="Release Type"
  multiple=true
  selectAllByDefault=true
>
    <DropdownOption valueLabel="Single" value="シングル" />
    <DropdownOption valueLabel="C/W" value="カップリング" />
    <DropdownOption valueLabel="Album" value="アルバム" />
</Dropdown>

```sql concert_songs_ranking
  select
      曲名 as song_title,
      収録回数 as dvd_record_count,
      タイプ as release_type
  from arashi_songs.arashi_songs
  where release_type in ${inputs.release_type_concert.value}
  order by dvd_record_count desc
  limit 20
```

<BarChart
  data={concert_songs_ranking}
  x=song_title
  y=dvd_record_count
  series=release_type
  swapXY=true
  yFmt=num0
  legend=false
/>

## Play count Top 20

<Dropdown 
  name=release_type
  title="Release Type"
  multiple=true
  selectAllByDefault=true
>
    <DropdownOption valueLabel="Single" value="シングル" />
    <DropdownOption valueLabel="C/W" value="カップリング" />
    <DropdownOption valueLabel="Album" value="アルバム" />
</Dropdown>

```sql play_count_ranking
  select
      曲名 as song_title,
      再生回数 as play_count,
      タイプ as release_type
  from arashi_songs.arashi_songs
  where release_type in ${inputs.release_type.value}
  order by play_count desc
  limit 20
```

<BarChart
  data={play_count_ranking}
  x=song_title
  y=play_count
  series=release_type
  swapXY=true
  yFmt=num0
  legend=false
/>