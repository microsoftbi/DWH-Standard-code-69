---
title: "Data Vault   \n  \nData Modeling Specification v 2.0.2 Data Vault数据建模规范V2.0.2"
---

译注
====

翻译此篇仅为个人爱好。从事数据仓库这些年，有幸能知道Data
Vault这个概念，并且在自己的项目中能够使用它，唯一遗憾的是对于Dan的这个规范文档看得一直是支离破碎。最近静下心来得以用读圣经得方式来读这个文档。当然在Data
Vault的知识体系中，这个也只能算是绝世武功的目录而已，所以我一直认为这个文档的地位，跟佛教的心经是一样的。

Translate this specification is just my favorite. I start to work in DWH area
for many years, and I am luck that I can know Data Vault, and even bring it to
my job. But it is really a pity that I always read it with small pieces.
Recently I start to read it just like to ready bible. Of course in whole Data
Vault knowledge base, this specification is only an index, but it is like the
Heart Sutra of Buddhism.

概要
====

New specifications for Data Vault 2.0 Methodology, Architecture, and
Implementation are coming soon... For now, I've updated the modeling
specification only to meet the needs of Data Vault 2.0. This document is a
definitional document, and does not cover the implementation details or “how-to”
best practices – for those, please refer to Data Vault Implementation Standards.

新的Data
Valut2.0方法论，架构以及实现马上就会到来。目前为止，我所更新这个建模规范仅仅是为了Data
Vault2.0的需要。这个文档只是一个定义性的文档，并不包括实现的细节，以及怎样去实现等最佳实践，这些内容请参考Data
Vault实现标准。

Please note: ALL of these definitions are taught in our Certified Data Vault 2.0
Practitioner course. They are also defined in the book: Building a Scalable Data
Warehouse with Data Vault 2.0 available on Amazon.com

请留意：所有这些定义都包含在了我们的Data
Vault2.0认证当中，这些内容可以在这本书中找到：Building a Scalable Data Warehouse
with Data Vault 2.0中。

These standards are FREE to the general public, and these standards are
up-to-date, and current. All standards published here should be considered the
correct and current standards for Data Vault Data Modeling.

这些标准对于公众是免费的，并且是持续更新的。所有发布在这里的标准对于Data
Vault数据建模的正确方法。

NOTE: tooling vendors: if you \*CLAIM\* to support Data Vault 2.0, then you must
support all standards as defined here. Otherwise, you are not allowed to claim
support of Data Vault 2.0 in any way. In order to make the public statement
“certified/Authorized to meet Data Vault 2.0 Standards” or “endorsed by Dan
Linstedt” you must make prior arrangements directly with me.

留意：对于工具提供商，如果在你的工具中宣布支持Data
Vault2.0，那么所有在这里定义的标准都必须被支持。否则，你不允许在你的工具中宣布支持Data
Vault2.0。如果你需要声明“符合Data Vault2.0标准的认证“或者”被Dan
Linstedt认可”的话，需要提前跟Dan联系。

实体类型定义
============

Hub实体
-------

A UNIQUE List of business Keys.

业务键的唯一列表

Please NOTE: This is the original and authentic definition of a Hub. Nowhere in
this definition have I ever stated that surrogates (either hashes or sequences)
are required for a Hub to work or to be in existence, yet people insist on
making this part of the base definition. It is NOT part of the base definition,
never has been, and never will be.

请留意：这是对Hub最原始的定义。在这个定义里没有任何位置我说明代理键（不管是哈希列或者是顺序列）对于Hub来说是必须的，也有人认为这个需要写入到基本定义中。这并不是基础定义的一部分，过去将来也不会。

Below is a Particularly Labeled Hub Type. There are no sub-classes of Hub
Entities.

以下是被特别标明的Hub类型。在Hub实体中没有子类型。

### 占位Hub(Stub Hub)

A Stub Hub is simply a Hub with a unique list of Business Keys. It is applied as
a place-holder for future expansion. It is a known business key, however the
source system(s) may be out of scope for the sprint or iteration, or the
Satellite details / descriptive data may be out of scope for the current build.

占位Hub也是一类基本Hub，主要目的是为了未来的扩展。有时对于业务键是确定的，但是对于源系统在当前迭代来说还不是在范围内，或者对应的Satellite详细描述信息在当前构建中不在范围。

A Stub Hub is not a class of Hub, as it is an application of a label for a
standard Hub Class.

Link实体
--------

A Stub Hub is not a class of Hub, as it is an application of a label for a
standard Hub Class.

两个或者多个业务键的关系的唯一列表。

A link is also known as: Transaction, Granularity Change, Hierarchical
Relationship, Recursive Relationship. Links are NEVER temporally based, they
never contain begin or end date attributes, the one exception: a non-historized
link may contain a transaction date.

Link也被认为：事务，粒度变化，层级关系以及递归关系。Link从来不是基于暂时的，从来不会包含开始和结束时间的属性，只有一个例外：不基于历史的Link可以包含业务时间。

NOTE: Links can never be Hubs. Links are always associative in nature and should
forever remain this way. This has to do with the basics of set logic
mathematics.

留意：Link不要作为Hub去用。Link应该一直是一种自然的联合。

Below is a list of Link Subclasses.

以下是Link的一些子类：

### 层级Link(Hierarchical Link)

A Hierarchical Link is a many-to-many recursive relationship structure, housing
a unique list of associations across two or more business keys at different
levels of data.

层级Link是多对多迭代关系结构，保存着对于数据不同层级跨越两个或者多个业务键的连接的唯一列表。

### 等同Link(Same-as Link)

A Same-As Link is a one (1) level deep hierarchy that maps similar terms
(synonyms) of business keys to a single master term or business key.

等同Link是一个在层级关系中只有一个层级的Link。

### Non-Historized Link 

A Non-Historized Link is a flattened denormalized structure containing immutable
facts. It can include the association keys, along with the immutable descriptive
data. For example: an immutable transaction, immutable event, or immutable fact
based data set that is never updated or altered.

### Exploration Link (Deep Learning Link) 

An exploration Link or a deep learning link is a standard link (see definition
of a Link entity above) built for a specific business purpose. The data set it
houses generally is created by business rules. Therefore, the exploration link
is a throw-away link. It can be built and discarded at will without affecting
auditability.

Its’ typical use is to contain two specially computed columns: strength and
confidence. Each instance (row) of data is measured by its’ neural network
correlation. Strength of the relationship should tell you (in percentage from
zero to 100%) how strong the correlation is between the associated data sets.
Confidence rating (from zero to 100%) should describe how confident the
decision-making engine is in its’ strength prediction. Additional attributes may
be added or attached to the Exploration Link in order to meet the needs of the
neural network or deep learning algorithms.

Satellites实体
--------------

Delta Driven Descriptive Information (data that changes over time).

增量驱动的描述性信息，随时间的变动而变动

Satellites are driven by CDC (change data capture) and split by rate of change
or type (classification) of data. Satellites contain the "data over time".

Satellite是受变更捕获驱动的，并且根据信息的变化频率或者信息的分类来拆分。Satellite包含的是随时间变化的数据。

Below is a list of Satellite Sub Classes

以下是Satellite的子类别

### Multi-Active Satellite 

A Multi-Active Satellite is a structure built to house multiple active child
records, where these child records have no stand-alone business key of their
own. A prime example would be an employee with multiple active addresses at the
same time.

Conditions that force a Multi-Active Satellite to be built include: bad quality
source data, lack of metadata on the source feed, lack of stand-alone business
key for the child records. In the example above, the addresses for each employee
do not have a properly formed unique business key.

### Effectivity Satellite 

An Effectivity Satellite is a standard Satellite housing temporal views of a
Link or Hub record. In other words, one or more sets of begin and end dates that
may dictate the effectivity of the relationship (Link record), or of the
business key (Hub Record).

Effectivity Satellites may also house deleted dates, or other metadata markers
that describe the parent records within or across timelines. NOTE: Physical
updates are never allowed on this data. See the Data Vault Implementation
Standards for more information.

### System Driven Satellite 

A system driven Satellite is a structure that is populated by hard business
rules and is driven by systematic or system regulated processes. A Point-in-Time
and a Bridge table are examples of system driven Satellites, they are built
strictly for performance reasons, and generally are populated with primary keys
and business key values from other Satellite structures. Another example of a
System Driven Satellite is a record source tracking Satelite.

System driven Satellites can be dropped and re-created for specific business
purposes without harming or affecting the underlying auditability of the data
sets.

其它表的分类Other Table Classifications 
========================================

独立表Stand-Alone Tables 
-------------------------

Calendar and time are prime examples of true reference tables. A stand-alone
table may be referred to or grouped with Reference Tables in the architecture,
however, they may also simply exist in the model. Stand-alone tables do not
exhibit any delta’s ever, nor do they house history, and typically their data is
immutable, and globally agreed upon.

日历表和时间表是独立表的典型示例。独立表也许会在架构中被引用表所引用或者分组，然而，他们在模型中也是存在的。独立表不需要任何增量方案，也不需要保留历史，并且通常数据都是不可改变的，并且在全球范围都是一致的。

报告表Reporting Tables 
-----------------------

A Reporting Table is a flat-wide denormalized structure, and generally the data
has been modified by soft business rules before loading. These reporting tables
generally live in the information mart layer of the architecture and are
leveraged for maximum performance. They can be imagined as a flattened cube, or
an excel spreadsheet represented in one large table structure.

报告表是扁平化的非常规化结构，在加载前通常也会进行一些软业务规则的计算。报告表在整个架构中，通常是在信息集市层来发挥其最大的性能。他们可以是扁平化的立方体，或者是excel里的一个大宽表。

Point in Time and Bridge Tables 
--------------------------------

Point in time table is a System Driven Satellite. It is comprised of primary key
values and business key values from a single Hub, and that Hubs’ surrounding
Satellites. It may also be comprised of a single Link and that Links’
surrounding Satellites. It is a snapshot table populated with snapshot-based
records of keys and key structures. It provides equal-join capacities to view
based Dimensions and view based Facts. It is built for performance of the
queries in getting data out of the Raw Data Vault. They are based on the
principles of Join Indexes as written by Teradata, only the point-in-time
structures can be implemented on ANY platform. Because Point-in-Time tables live
within the Information Mart logical construct they can also house computed
fields, and / or additional temporality (such as begin and end dates that have
been computed for business purposes).

The Bridge Table is a combination of primary keys and business keys spread
across multiple Hubs and Links. They can be thought of as “base level Fact
Tables”. They provide a snapshot of key structures and are generally not
temporal in nature. That said, because Bridge Tables live within the Information
Mart logical construct, they can also house computed fields, and / or
temporality.

引用表Reference Tables 
-----------------------

Reference tables are a logical collection of code and description lookup
structures. They utilize natural business keys, and are constructed from
standard Hub, Link, and Satellite entities.

引用表是代码和描述信息的集合。他们使用自然业务键，并且从Hub，Link以及Satellite实体中构建。

Resolution occurs through queries at run time. They do not house nor require
foreign keys. In general, the codes (natural keys) are found housed in the
Satellites as they typically describe other keys or other relationships.

计息发生在在实际的查询中当红[这句不是很理解]。他们不会保留或者使用外键。通常来说，代码（自然键）存在于Satellite中，正如他们也是用来描述其它的键或者关系的。

集结表Staging Tables 
---------------------

Staging tables are a logical collection of structured data sets. They are
generally applied to house data that is loaded to a particular database
platform. Staging tables have two main purposes: a) to provide performance with
bulk loaders, b) to provide parallelism on loads to the Data Vault (raw data
warehouse) downstream.

集结表是结构化数据的集合。他们通常是从一个常规的数据仓库平台加载过来的。集结表的设计有两个目的，一个是供bulk
load发挥性能，一个是提供往下一层Data Vault并行加载。

On occasions where bulk loaders cannot compute the system fields on load, a
second level staging table may be built. The second level staging table then
houses the primary key of the staging record, followed by the computed system
fields (such as Hash Keys, Hash Differences, Load Dates, and record sources).

有时bulk
load无法在加载的过程中进行计算，这个时候可以考虑再加一层集结表，这一层的表仍然是需要保留来自上一层的主键，以及计算列，包括哈希键，哈希差异，加载时间以及记录源。

Note: for details on the loading process see the Data Vault Implementation
Standards guidelines.

留意：关于加载流程的更多信息请参考Data
Vault实现规范指导。（译者注：这个文档直到今天也没有被找到。）

着陆区文件（在NoSql环境中）Landing Zone Files (in NoSQL environment) 
---------------------------------------------------------------------

Landing Zone Files are simply files that have been loaded or dumped into a
Hadoop base environment, or other alternative metadata managed storage
environment. A metadata managed storage environment has the following
characteristics: parallel access, partitioning, sharding, and possibly
sub-partitioning of the data, where the metadata is registered with an access
engine (similar to Hadoop).

着陆区的文件通常指加载到Hadoop环境的文件，或者其它元数据托管的存储环境，这种环境通常有以下特征：并行访问，分区，分片，并且可以做子分区，它们的元数据也是可以被访问到的，就像Hadoop一样。

公共字段元素Common field elements 
==================================

Common field elements are SYSTEM DRIVEN, and SYSTEM Managed – they do \*NOT\*
fall under the scrutiny of an audit. They are generated fields on the way IN to
the target (stage, data vault, or star schema) and are necessary for assisting
in the traceability of individual fields.

公共字段元素是系统生成，系统管理的，它们的目的不是为了安全或者审计。他们是到目标区域，比如集合层，Data
Vault层或者星型结构的派生列，也是为了辅助单独的列辅助追钟的必要方法。

哈希键（可选列）Hash Key (Optional \*\* see note below) 
--------------------------------------------------------

A Hash Key is a deterministic value produced by a given hashing function. The
requirement is to leverage the business key attribute(s) as input and produce a
deterministic output. Due to the nature of deterministic calculation data can be
loaded in parallel without any foreign key lookups (which is what Sequence
identifiers require in order to resolve parent child relationships).

哈希键是一种根据给定哈希函数计算后得到的一个不可逆的值。这个过程是利用输入的业务键（或者复合业务键），然后输出一个不可逆的值。这种方法不需要进行外键查找，所以可以实现加载的并行。（顺序标识位是需要外键查找来解决父子层级关系的。）

The purpose of the Hash Key is not to be encrypted and not to protect the data.
However there are a series of encryption functions which produce hash-like
values that may be utilized. This is particularly helpful when the clear-text
data cannot leave physical boundaries or data stores (either in country, or on
premise) due to security and privacy regulations.

哈希键的目的不是为了加密数据也不是为了保护数据。确实有些加密函数跟哈希计算很像，这在明文数据根据安全以及隐私条例不能保存的时候能起到一定作用，比如在国家安全类项目或者本地项目。

Due to the deterministic nature of Hashing, most hashing algorithms can be
applied for average equal random distribution across multiple partitions or
shards. In other words, load balancing the data sets.

NOTE: Hash Keys are OPTIONAL on platforms like Teradata that support internal
hashing of natural business keys. If a platform can function and hashes the
natural keys (including potentially long text fields) internally, then an
externally declared Hash Key is no longer required. \*\*\* Remember, the core
definition of a Hub is: Unique List Of Business Keys \*\*\*

留意：哈希键在某些平台下是不是必须的，比如Teradata，其本身就带这种哈希计算的业务键列。所以如果一个平台如果已经有了类似的功能，是没有必要创建一个额外的哈希键的。\*\*\*请记住，Hub的核心定义是：业务键的唯一序列\*\*\*

NOTE 2: Some Business keys (not necessarily natural keys) are sequence numbers,
for example: Invoice Number, and will never change regardless of the business
position. It is possible in these cases to avoid using Hash Keys, and simply
leverage the source business key in its native format. That said, even in these
cases – a sequence number surrogate key is not recommended. In fact, sequence
number surrogate keys have been deprecated due to their inability to scale. \*\*
See the notes below for further information \*\*

留意2：有些业务键（不一定是自然键）是一种序列数值，比如，发票编号，从业务的角度来说这个值是从来不会变化的。像这种情况就可以不使用哈希键，而是直接使用这个业务键。也就是说，即使在这类情况下，顺序编号的代理键也是不被建议的。并且实际上，顺序编号的代理键由于它们无法被扩展所以不是被推荐的。

NOTE 3: Hash results should be stored in binary format in platforms that support
it, and in big number format in platforms that support it. Java, and platforms
like SnowflakeDB support number(32) unsigned. Which means converting MD5 output
to a number(32) unsigned is allowed, and encouraged. In SnowflakeDB use
MD5_Number( x ) to convert.

留意3：哈希结果，如果平台支持二进制格式保存，那么就用二进制格式保存，如果平台支持用大数值类型保存，那么就用大数值类型。Java以及类似SnowflakeDB的平台是支持number(32)无符号类型的。也就是说转换MD5输出成number(32)无符号类型是可以并且也被鼓励的。在SnowflakeDB平台用MD5_Number(x)来进行转换。

NOTE 4: IF a hashing value will be utilized then it is required to design and
document a hash collision strategy (see Wikipedia for birthday problem, and hash
collision discussions).

NOTE 5: IF Business Keys are chosen, then a strategy for dealing with
multi-field joins must be devised, particularly to overcome multi-field join
performance issues. Generally this means concatenating multiple business key
pieces (composite pieces) together to arrive at a single field join. See Data
Vault Implementation Standards for more details.

Business Key (Required) 
------------------------

A Business Key is defined to be the same semantic grain and same semantic
meaning across multiple lines of business. If the grain is different, or the
semantic meaning changes (regardless of the value), then separate Hub structures
are created.

A Business Key may be defined as a source system surrogate identifier (sequence
number from a source system). This happens when the value is shown to the
business users, it is at that point that the business begins to leverage the
value as THE unique identifier for the records.

NOTE: in a \*perfect\* world, we would construct Hub structures with JUST the
business key which in effect would turn the Hubs in to indexing structures.
Again, see Data Vault

Implementation Standards for best practices as to how, when, and why to utilize
business keys (instead of hash keys or EDW sequences) in different environments.

Hash Difference (Optional) 
---------------------------

A Hash Difference is a computed field value based upon concatenation of
descriptive attributes applied to a satellite and pushed through a hashing
function. Instead of comparing each column (column by column) to determine a
delta, the hash difference attribute can be compared. If they differ – a delta
for the Satellite has been found.

This particular field is not necessary in database engines such as Teradata –
due to the massive block size and high parallelism of the query engine, Teradata
can compare many columns just as quickly as a predetermined Hash Difference
column. That said, most other platforms benefit (performance wise) from
utilizing a Hash Difference for comparison and delta checking purposes.

Load Date Time Stamp (Optional) 
--------------------------------

The Load Date Time Stamp is the date and time of insert for the record.
Obviously in data management platforms like Hadoop HDFS, no such concept of
record exists. Therefore, this field is now optional. (see the note below)

An attribute in the Hub, and Link – part of the primary key in a relational
Satellite. This is the date stamp of the date/time that the data was loaded into
the database. This is stamped this way for consistency of information across the
relational database. The Load Date Time Stamp is never a part of the primary key
of a Hub or a Link.

In a real-time solution the load-date may not be dependable unless the
transactions can be guaranteed to be inserted in the order in which they were
created. \*\* See APPLIED DATE below \*\*

NOTE: It is REQUIRED for Relational Database Engines, however it is not possible
to apply to Hadoop Files. In reality, Hadoop stamps the file at the file level
in HCatalog with the date and time the file was “ingested”. This is as close as
the data can get to a Load-Date. Hence it is now optional.

Record Source (Required) 
-------------------------

This is the source system that generated the information, it is mechanically
stamped when the information is loaded to the database. Used when there is no
meta-data project in place to trace information back to the source. It provides
tracability of every record at a granular level back to the source system. While
optional, it is implemented by 98% of the customers today.

Update User (optional) 
-----------------------

This is there to track DBA level modifications to the data. It is optional and
should be in another metrics tracking area. Typically found in the Metrics Vault
component. Update User should never appear in any of the data sets inside the
Raw Data Vault, as it requires a physical hard update against any records that
are being modified. Physical raw updates do not scale (in performance numbers)
to massive data set sizes.

Update Time Stamp (optional) 
-----------------------------

This is another DBA tracking field. It also is optional and should be in another
metrics tracking area. Typically found in the Metrics Vault component. Update
Time Stamp should never appear in any of the data sets inside the Raw Data
Vault, as it requires a physical hard update against any records that are being
modified. Physical raw updates do not scale (in performance numbers) to massive
data set sizes.

Extract Date (Optional) 
------------------------

This has proven to be beneficial if included in the module. There are times at
which knowing what the extract date is, helps. Extract Dates should always be
additional attributes in Hubs, Links, and Satellites. They should never be
applied as load dates.

Applied Date Time Stamp (OPTIONAL) 
-----------------------------------

Applied Dates are date time stamps which can be manipulated to indicate when in
the business timeline the data applies. Applied Date Time Stamps can appear in
Hubs, Links, and Satellites. In other words, instead of setting or changing load
dates, instead of leveraging extract dates, the Applied Date can be set
according to business needs. In the case of real-time feeds, an Applied Date may
be the transaction creation date. In the case of historical batch feeds, it may
be set to the “back-dated date” that represents when in history the data
applies.

Applied dates are not the same as load dates. Applied Dates are to be leveraged
by SQL queries, aggregates, and business applications.

Hub Rules 
==========

Business Key 
-------------

A Hub must have at least one (1) business key driven field. (See definition of
Business Key Above)

Business Key to Surrogate 
--------------------------

A Hub’s business key must be one to one with the surrogate \*IF a surrogate
(hash key or sequence) is utilized

Multiple Distinct Business Keys 
--------------------------------

A Hub cannot contain a composite set of multiple business keys that are
stand-alone. For instance: Account Number and Invoice Number, these two business
keys are clearly stand-alone keys, and must be modeled each in their own Hub
with a Link association between them.

Hub Satellite (Optional) 
-------------------------

A Hub should support at least one satellite to be in existence, Hubs without
Satellites usually indicate “bad source data”, or poorly defined source data, or
business keys that are missing valuable metadata. Hubs without any Satellites
have no context.

Hub Composite Key Attributes 
-----------------------------

A single Hub business Key can be composite when: two of the same operational
systems are using the same keys to mean different things AND these keys collide
when integrated back together again.

Please be aware: BAD DATA CAUSES BREAKS IN THESE RULES – THESE ARE GUIDING

PRINCIPLES. Exceptions to this rule should not happen (but do), also be aware,
bad architecture in source systems causes breaks in these rules too.

Hub Business Key Stands Alone 
------------------------------

A Hub’s business key must stand-alone in the environment – either be a system
created key, or a true business key that is the single basis for “finding”
information in the source system. A True business key is often referred to as a
NATURAL KEY.

Alternative source business key may be a sequence id. The minute a sequence on
the source system is shown to the business user, is the minute it becomes a
business key on the source system. It is at this point it qualifies as a
business key in the Hub.

Hub Load Date Time Stamp 
-------------------------

A Hub’s load-date-time stamp or observation start date must be an attribute in
the hub, and never part of the hub’s primary key structure. A Hubs’ Load Date
represents the first time the data was inserted to the Hub.

Hub Record Source 
------------------

A Hub’s PRIMARY KEY cannot contain a record source. Hub Record source indicates
the first system to insert the data.

Link Rules 
===========

A Link is an Intersection of Hubs in a Venn Diagram 
----------------------------------------------------

A Link must contain two (2) or more imported Hub primary keys. A Link can never
depend on another link.

Hierarchical Link Relationship 
-------------------------------

A Link can contain two keys imported from the same hub for a hierarchical
relationship or rolled up relationship. A HIERARCHICAL representation of a
relationship or aggregation across a single hub’s key, migrated in exactly two
times.

Any further hierarchy is broken down into two migrations, this way no limitation
is placed upon the hierarchy, and the Link is NOT playing a role-game which is
dangerous. Also, a Hierarchical Link must contain at least one Satellite to
indicate effectivity of the relationship (start and end dating of the
hierarchical relationship.

Link Load Date (Required) 
--------------------------

A Link’s load-date-time stamp or observation start date must be an attribute in
the link, and never a part of the links’ primary key structure.

Link Composite Key 
-------------------

A Links composite key must be unique (A unique business key). The Links’
composite key is comprised of all business keys or hashes from parent Hub
Structures.

Link Surrogate Key (Optional) 
------------------------------

A Link may contain a surrogate hash key (if the composite is too large, or the
database doesn’t work well with natural keys). If a Surrogate Hash Key is built,
it will be the only attribute in the primary key of the Link. Note:
Non-Historized Links (transactional links) do not require a Link Surrogate Key
since they never have any child Satellites built.

Link Granularity 
-----------------

A Links’ granularity is determined by the number of imported Hub parent keys.

Link May be Defined as 
-----------------------

A Link is a transaction, or a hierarchy, or a relationship.

Link Satellites are Optional 
-----------------------------

A Link may have zero or more Satellites attached.

Links and Temporality 
----------------------

Links never contain begin or end dates. These are always assigned to exist in
Satellite Effectivity. The sole exception: non-historized Link. (See the
definition of Link Section 1.2 above)

Link Granularity 
-----------------

A Link must be at the lowest level of granularity for tracking purposes.

Link Data Representation 
-------------------------

A Link must represent at most, 1 instance of a relationship component for ALL
time.

Link Driving Key Rule 
----------------------

In a Link, the consistent part of the key is the primary driver for the
relationship.

Satellite Rules 
================

Satellite Parent Key 
---------------------

A Satellite must = have at least one Hub or Link primary key imported. A
Satellite can never generate its’ own primary key. A Satellite can never
maintain or create its’ own surrogate sequence key or its’ own hash key.

Satellite Single Parent ONLY 
-----------------------------

A Satellite cannot be attached to more than one Hub or Link parent structure. A
Satellite can

NEVER EVER be snow flaked (in accordance with snowflake definitions provided by
Kimball Star Schemas)

Satellite Load Date Time Stamp (Required) 
------------------------------------------

A Satellite MUST have a load-date-time stamp (observation start date) as a part
of its primary key.

Satellite Sub-Sequence (Optional) 
----------------------------------

A Satellite may contain a sub-sequence identifier or and ordering identifier as
part of the primary key for uniqueness. This additional sub-sequence can be a
microsecond counter, or an ordering attribute (as in the case of a multi-active
Satellite record)

Satellite Record Source (Required) 
-----------------------------------

A Satellite must contain a record source attribute for data traceability.

Satellite Descriptive Elements (Required) 
------------------------------------------

A Satellite must contain at least one descriptive element about the Hub or Link
to which it’s attached in order to be valid. This rule includes effectivity and
system driven Satellites.

Business Satellite Data 
------------------------

A Business Vault Satellite may contain system generated or aggregated attributes
as a result of soft rule calculations.

Satellite Purpose 
------------------

A Satellite’s purpose is to store data over time.

Satellite Reference Table Access 
---------------------------------

A Satellite may contain a natural key (i.e. code) to a stand-alone
code/description table. FK’s to reference tables are implied – never physically
implemented. Indirect references to date time calendar table, or geography is
acceptable. These foreign keys are NOT to be represented within the physical
data model.

Satellite Split or Merge Actions 
---------------------------------

A Satellite may be split or merged at any time, as long as NO HISTORICAL VALUE
is lost, and NO HISTORICAL AUDIT TRAIL is lost. See the Data Vault
Implementation Standards for rules and processes around how-to execute a split
or merge.

Naming Conventions 
===================

Naming conventions are enforced in order to meet the needs automation and
standardization.

Without naming conventions, the models will get out of hand and become
unmanageable.

Data Vault models which do not follow documented naming conventions cannot be
automated.

The conventions proposed below are recommendations, not requirements. What is
required is to create and maintain a naming convention document for every Data
Vault Model that is designed.

Entity Naming Conventions 
--------------------------

| Entity Type        | Prefix or Suffix      |
|--------------------|-----------------------|
| Hub                | **H, HUB, HB**        |
| Link               | **L, LINK, LNK**      |
| Satellite          | **S, SAT**            |
| Stage              | **STG**               |
| Hierarchical Links | **HL, HLNK, HLINK**   |
| Same-As Links      | **SAL, SALNK, SLNK**  |
| Point-in-Time      | **PIT, PT**           |
| Bridge             | **B, BRDG, BRG**      |
| Business Hub       | **BH, BHUB**          |
| Business Link      | **BL, BLNK, BLINK**   |
| Business Satellite | **BS, BSAT, BST**     |
| View               | **V**                 |
| View Dimension     | **VDIM, VD**          |
| View Fact          | **VF, VFCT**          |
| Fact               | **FCT, FACT, F**      |
| Dimension          | **D, DIM**            |
| Report Collection  | **RPT, RC**           |

Field Naming Conventions 
-------------------------

| Attribute Type            | Prefix or Suffix              |
|---------------------------|-------------------------------|
| Record Source             | **R, RSRC, RS**               |
| Sequence ID               | **SQN, SEQ**                  |
| Date Time Stamps          | **DTS, DTM**                  |
| Date Stamps               | **DS, DT**                    |
| Time Stamps               | **TMS, TS, TM**               |
| Load Date Time Stamps     | **LDTS, LDDTS, LDTM**         |
| User Watch Fields         | **USR, U**                    |
| Occurrence Numbers        | **OCC, OCNUM, ONUM**          |
| Load End Date Time Stamps | **LEDTS**                     |
| Sub Sequence              | **SSQN, SSQ, SUBSQN**         |
| Applied Dates             | **APPDT, ADT, APDT**          |
| Hash Keys                 | **HK, HashKey, HKEY**         |
| Hash Differences          | **HD, HashDiff, HDIFF, HDF**  |

DEPRICATED RULES 
=================

These rules have lost their ability to be effective in the Data Vault 2.0
Landscape due to big data (volume), platform issues, velocity of data arrival,
or variety of data sets. Therefore, these rules are deprecated / removed from
the standard, and should never be utilized going forward.

Hub and Satellite Sequence ID (DEPRECATED) 
-------------------------------------------

WARNING SURROGATE SEQUENCE BASED IDENTIFIERS SHOULD NO LONGER BE UTILIZED. Why
has it been deprecated? For the following reasons:

• It requires lookups on a row by row basis for resolution. These lookups cannot
scale in to the hundreds of terabytes or petabyte ranges of data movement and
integration. These lookups are sequential processes

• Sequence ID's are non deterministic. Requiring "hybrid" or geographically
split platforms to perform lookup operations across multiple database instances.
Lookups across geographic splits, or hybrid (in-cloud / on-premise) solutions
are not fully scalable, and do not perform to the expectations set by the
business today.

• Lookups across multiple country boundaries by clear-text business keys are
noncompliant with data protection and privacy regulations, which make the
lookups infeasible to utilize.

• Being non-deterministic, means they are not good at: uniquely identifying
(consistently and deterministic identifying): text, documents, video, audio, or
image files in parallel environments at the same time.

Satellite Load End-Dates (DEPRECATED) 
--------------------------------------

Load End dates are no longer allowed in the RAW data Vault, as they require
physical updates which a) are not supported by all platforms, b) do not scale
properly over hundreds of terabytes of data. Also, load end dates are not
consistent in real-time inflow of data sets because data may arrive out of order
from when it was created – causing multiple problems with load end dates.

Note: If End-Dates are truly necessary, use Lead/Lag functions to compute them,
and instantiate their values in the Point in Time and Bridge Table structures.
See Data Vault Implementation Standards for more information.

Hub and Link Last Seen Dates (DEPRECATED) 
------------------------------------------

Last seen dates are no longer allowed, as they require physical updates which a)
are not supported by all platforms, b) do not scale properly over hundreds of
terabytes of data.
