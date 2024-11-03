---
title: All Songs Details
---

```sql release_year
  select
      リリース年 as release_year
  from arashi_songs.arashi_songs_20241103_0639
  group by release_year
  order by release_year
```

<Dropdown
    name=release_year_filter
    title="Release Year"
    data={release_year}
    value=release_year
    multiple=true
    selectAllByDefault=true
>
</Dropdown>

<Dropdown 
  name=release_type_details
  title="Release Type"
  multiple=true
  selectAllByDefault=true
>
    <DropdownOption valueLabel="Single" value="シングル" />
    <DropdownOption valueLabel="C/W" value="カップリング" />
    <DropdownOption valueLabel="Album" value="アルバム" />
</Dropdown>


<!--
<TextInput
    name=songwriter_name
    title="Songwriter Search (Not Working)"
    placeholder="Songwriter name"
/>
-->


```sql details
  select
      曲名 as song_title,
      SPLIT_PART(収録CD, ' | ', 1) as title,
      リリース年 as release_year,
      再生回数 as play_count,
      収録回数 as concert_record_count,
      タイアップ、備考 as remarks,
      クレジット as songwriter
  from arashi_songs.arashi_songs_20241103_0639
  where 
    タイプ in ${inputs.release_type_details.value} and
    release_year in ${inputs.release_year_filter.value}
```

<DataTable data={details} rows=all/>

