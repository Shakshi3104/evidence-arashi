---
title: All Songs Details
---

```sql release_year
  select
      リリース年 as release_year
  from arashi_songs.arashi_songs
  group by release_year
  order by release_year
```

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

<Dropdown
    name=release_year_filter
    title="Release Year"
    data={release_year}
    value=release_year
    multiple=true
    selectAllByDefault=true
>
</Dropdown>


Options for finding songs with written by ARASHI members.

<ButtonGroup 
    name=songwriter_arashi_options 
>
    <ButtonGroupItem valueLabel="All" value="%" default />
    <ButtonGroupItem valueLabel="櫻井翔" value="櫻井翔" />
    <ButtonGroupItem valueLabel="二宮和也" value="二宮和也" />
    <ButtonGroupItem valueLabel="嵐" value="嵐" />
</ButtonGroup>





```sql details
  select
      曲名 as song_title,
      リリースタイトル as title,
      リリース年 as release_year,
      リリース日 as release_date,
      再生回数 as play_count,
      収録回数 as concert_record_count,
      タイアップ、備考 as remarks,
      クレジット as songwriter,

      replace(replace(クレジット, 'SHOW', '櫻井翔'), 'Sho Sakurai', '櫻井翔') as songwriter_search

  from arashi_songs.arashi_songs as songs
  left join arashi_songs.all_releases as releases
    on SPLIT_PART(songs.収録CD, ' | ', 1) = releases.リリースタイトル

  where 
    songs.タイプ in ${inputs.release_type_details.value} and
    release_year in ${inputs.release_year_filter.value} and
    songwriter_search like '%${inputs.songwriter_arashi_options}%'

  order by
    column0, release_date
```

<DataTable data={details} rows=all search=true >
    <Column id=song_title title="Title" />
    <Column id=title title="Release" />
    <Column id=release_date title="Release Day" />
    <Column id=play_count title="Play Counts" contentType=bar />
    <Column id=concert_record_count title="Live DVD Counts" contentType=bar />
    <Column id=remarks title="Remarks" />
    <Column id=songwriter title="Songwriters" />
</DataTable>

