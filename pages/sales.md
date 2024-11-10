---
title: Sales
---

<Dropdown 
  name=release_type
  title="Release Type"
  defaultValue="All"
>
    <DropdownOption valueLabel="All" value="All" />
    <DropdownOption valueLabel="Single" value="Single" />
    <DropdownOption valueLabel="Album" value="Album" />
    <DropdownOption valueLabel="DVD/Blu-ray" value="DVD/Blu-ray" />
</Dropdown>

```sql total_sales
  select
    SUM(売上枚数) as sales_count

  from arashi_songs.all_releases
```

```sql single_sales
  select
    SUM(売上枚数) as sales_count

  from arashi_songs.all_releases
  where タイプ like '%シングル%'
```

```sql album_sales
  select
    SUM(売上枚数) as sales_count

  from arashi_songs.all_releases
  where タイプ like '%アルバム%'
```

```sql dvd_sales
  select
    SUM(売上枚数) as sales_count

  from arashi_songs.all_releases
  where タイプ like '%DVD%' or タイプ like '%Blu-ray%'
```

<BigValue
  data={
    inputs.release_type.value == 'All' ? total_sales : 
    inputs.release_type.value == 'Single' ? single_sales :
    inputs.release_type.value == 'Album' ? album_sales :
    dvd_sales
    }
  value=sales_count
  title='{inputs.release_type.value} Sales'
  fmt=num0
/>

## {inputs.release_type.value} Sales Change

```sql all_sales_change
  select
    リリースタイトル as title,
    リリース日 as release_day,
    format('{}\n({})', title, strftime(release_day, '%Y/%m/%d')) as title_release_day,
    SUM(売上枚数) as sales_count

  from arashi_songs.all_releases
  where 売上枚数 is not NULL
  group by リリースタイトル, リリース日
  order by リリース日
```

```sql single_sales_change
  select
    リリースタイトル as title,
    リリース日 as release_day,
    format('{}\n({})', title, strftime(release_day, '%Y/%m/%d')) as title_release_day,
    SUM(売上枚数) as sales_count

  from arashi_songs.all_releases
  where 売上枚数 is not NULL
    and タイプ like '%シングル%'
  group by リリースタイトル, リリース日
  order by リリース日
```

```sql album_sales_change
  select
    リリースタイトル as title,
    リリース日 as release_day,
    format('{}\n({})', title, strftime(release_day, '%Y/%m/%d')) as title_release_day,
    SUM(売上枚数) as sales_count

  from arashi_songs.all_releases
  where 売上枚数 is not NULL
    and タイプ like '%アルバム%'
  group by リリースタイトル, リリース日
  order by リリース日
```

```sql dvd_sales_change
  select
    リリースタイトル as title,
    リリース日 as release_day,
    format('{}\n({})', title, strftime(release_day, '%Y/%m/%d')) as title_release_day,
    SUM(売上枚数) as sales_count

  from arashi_songs.all_releases
  where 売上枚数 is not NULL
    and (タイプ like '%DVD%' or タイプ like '%Blu-ray%')
  group by リリースタイトル, リリース日
  order by リリース日
```

<BarChart
    data={
        inputs.release_type.value == 'All' ? all_sales_change : 
        inputs.release_type.value == 'Single' ? single_sales_change :
        inputs.release_type.value == 'Album' ? album_sales_change :
        dvd_sales_change
    }
    x=title_release_day
    y=sales_count
    sort=false
>
    <!-- <ReferenceArea xMin='1999-11-03' xMax='2001-11-12' label='PONY CANYON' color=red />
    <ReferenceArea xMin='2001-11-12' xMax='2025-01-01' label='Storm Lables (J Storm)' /> -->
</BarChart>

## {inputs.release_type.value} Sales Top 10

```sql all_sales_top10
  select
    リリースタイトル as title,
    リリース日 as release_day,
    format('{} ({})', title, strftime(release_day, '%Y/%m/%d')) as title_release_day,
    SUM(売上枚数) as sales_count

  from arashi_songs.all_releases
  where 売上枚数 is not NULL
  group by リリースタイトル, リリース日
  order by sales_count desc
  limit 10
```

```sql single_sales_top10
  select
    リリースタイトル as title,
    リリース日 as release_day,
    format('{} ({})', title, strftime(release_day, '%Y/%m/%d')) as title_release_day,
    SUM(売上枚数) as sales_count

  from arashi_songs.all_releases
  where 売上枚数 is not NULL
    and タイプ like '%シングル%'
  group by リリースタイトル, リリース日
  order by sales_count desc
  limit 10
```

```sql album_sales_top10
  select
    リリースタイトル as title,
    リリース日 as release_day,
    format('{} ({})', title, strftime(release_day, '%Y/%m/%d')) as title_release_day,
    SUM(売上枚数) as sales_count

  from arashi_songs.all_releases
  where 売上枚数 is not NULL
    and タイプ like '%アルバム%'
  group by リリースタイトル, リリース日
  order by sales_count desc
  limit 10
```

```sql dvd_sales_top10
  select
    リリースタイトル as title,
    リリース日 as release_day,
    format('{} ({})', title, strftime(release_day, '%Y/%m/%d')) as title_release_day,
    SUM(売上枚数) as sales_count

  from arashi_songs.all_releases
  where 売上枚数 is not NULL
    and (タイプ like '%DVD%' or タイプ like '%Blu-ray%')
  group by リリースタイトル, リリース日
  order by sales_count desc
  limit 10
```

<BarChart
    data={
        inputs.release_type.value == 'All' ? all_sales_top10 : 
        inputs.release_type.value == 'Single' ? single_sales_top10 :
        inputs.release_type.value == 'Album' ? album_sales_top10 :
        dvd_sales_top10
    }
    x=title_release_day
    y=sales_count
    swapXY=true
/>

## Details

```sql all_releases_details
  select
    リリースタイトル as title,
    タイプ as release_type,
    リリース日 as release_day,
    型番 as product_id,
    売上枚数 as sales_count

  from arashi_songs.all_releases
```

```sql single_releases_details
  select
    リリースタイトル as title,
    タイプ as release_type,
    リリース日 as release_day,
    型番 as product_id,
    売上枚数 as sales_count

  from arashi_songs.all_releases
  where タイプ like 'シングル'
```


```sql album_releases_details
  select
    リリースタイトル as title,
    タイプ as release_type,
    リリース日 as release_day,
    型番 as product_id,
    売上枚数 as sales_count

  from arashi_songs.all_releases
  where タイプ like 'アルバム'
```


```sql dvd_releases_details
  select
    リリースタイトル as title,
    タイプ as release_type,
    リリース日 as release_day,
    型番 as product_id,
    売上枚数 as sales_count

  from arashi_songs.all_releases
  where タイプ like '%DVD%' or タイプ like '%Blu-ray%'
```

<DataTable data={
    inputs.release_type.value == 'All' ? all_releases_details : 
    inputs.release_type.value == 'Single' ? single_releases_details :
    inputs.release_type.value == 'Album' ? album_releases_details :
    dvd_releases_details
    } 
    rows=all search=true >
    <Column id=title title="Title" />
    <Column id=release_day title="Release" />
    <Column id=release_type title="Type" />
    <Column id=product_id title="Product ID" />
    <Column id=sales_count title="Sales" contentType=bar />
</DataTable>