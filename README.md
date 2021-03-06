sparktool
========
built to simplified the process of creating sparksession(hccn).
* get saved query from hue
* batch execute sqls
* parse impala/hive view automatically
* parse kudu table automatically


install
=======
Python 2/3 

* `pip install --user --upgrade sparktool`
* `pip2 install --user --upgrade sparktool`


functions
=======

switch_keytab (first run)
--------
```
import sparktool as st
st.switch_keytab('admin@EXAMPLE.COM', keytabpath)
```

switch_huetab (first run)
--------
```
import sparktool as st
st.switch_huetab(username, password)
```

SparkCreator
--------
[in]

```python
import sparktool as st
ss = st.SparkCreator()
sql = '''
select 1;
select
    cc.skp_client
from
    owner_ogg.ft_ccase_2_ccase_ad cc
    join owner_ogg.clt_ccase_2_ccase_relation re
      on cc.skp_ccase_2_ccase_relation = re.skp_ccase_2_ccase_relation
     and re.code_ccase_relation = 'FIRST_POS' 
limit 1
;
'''
ss.batch_excutesql(sql)
```

[out]

```python
Tranform Table:
+--------------------------------------+--------------------------------------+--------------+
|             Origin Table             |            Temporary View            | If Transform |
+--------------------------------------+--------------------------------------+--------------+
|    OWNER_OGG.FT_CCASE_2_CCASE_AD     |    owner_ogg_ft_ccase_2_ccase_ad     |     New      |
| OWNER_OGG.CLT_CCASE_2_CCASE_RELATION | owner_ogg_clt_ccase_2_ccase_relation |     New      |
+--------------------------------------+--------------------------------------+--------------+
Excute Progress: 2/2
DataFrame[skp_client: decimal(38,0)]
```

[param]
* ifview: if view in sql, it will parse view to code
* ifbatchre: b,c,d,e,f = ss.batch_excutesql(sql, ifview=True, ifbatchre=True)

HueCreator
--------
[in]
```python
from sparktool import HueCreator
aa = HueCreator()
aa.hue_printlist()
```
[out]
```python
+--------+-------------------------+----------------------------+-------------------+
|   id   |           name          |        description         |   last_modified   |
+--------+-------------------------+----------------------------+-------------------+
| 691124 |     aaaaaaaaaaaaaaa     |                            | 2020-01-17T17:41Z |
| 686849 |           hcp           |                            | 2020-01-17T10:32Z |
| 675390 |           aaaa          |                            | 2020-01-16T11:05Z |
| 681235 |          ttttt          |                            | 2020-01-15T09:41Z |
| 676699 |           aaaa          |                            | 2020-01-14T09:53Z | |
+--------+-------------------------+----------------------------+-------------------+
```

[in]
```python
aa.hue_getscript('hcp')
```
[param]
* ifprint: print sql which is beautified

[in]
```python
aa.hue_setscript(sql, name='hcp', uuid=None)
```
[param]
* uuid > name

idea
--------
https://github.com/renyumm/sparktool/blob/master/Sparkcreator.png