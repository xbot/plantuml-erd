```plantuml
@startuml

!include https://raw.githubusercontent.com/xbot/plantuml-erd/master/src/Library.iuml

' avoid problems with angled crows feet
skinparam linetype ortho

''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''
' Entities
'
' Relationships
' one-to-one relationship
'     user -- user_profile : "A user only \nhas one profile"
' one to may relationship
'     user --> session : "A user may have\n many sessions"
' many to many relationship
' Add mark if you like
'     user "1" --> "*" user_group : "A user may be \nin many groups"
'     group "1" --> "0..N" user_group : "A group may \ncontain many users"
''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''

Table(posts, 文章表) {
    PRIMARY_KEY
    TIMESTAMPS
    Column("title", "NVARCHAR[30]", 1, "", "标题")
    Column("status", UN("TINYINT"), 1, "1", "状态", "1:草稿; 2:已发布")
    Column("source", UN("TINYINT"), 1, "", "来源：1，原创；2，转载")
    Column("category_id", PK_TYPE, 1, "", "分类")
    Column("created_by", PK_TYPE, 1, "0", "创建者ID")
    Column("published_at", "DATETIME", 0, "", "发表时间")
    Column("content", "TEXT", 1, "", "内容")
}

Table(comments, 评论表) {
    PRIMARY_KEY
    TIMESTAMPS
    Column("post_id", PK_TYPE, 1, "", "文章ID")
    Column("user_id", PK_TYPE, 1, "", "用户ID")
    Column("content", "NVARCHAR[255]", 1, "", "评论内容")
}

Table(post_logs, 文章日志表) {
    PRIMARY_KEY
    TIMESTAMPS
    Column("post_id", PK_TYPE, 1, "", "文章ID")
    Column("user_id", PK_TYPE, 1, "", "操作者ID")
    Column("action", UN("TINYINT"), 1, "0", "操作类型", "1:创建; 2:修改; 3:删除")
    Column("data", "TEXT", 0, "", "操作详情")
}

posts ||..o{ comments
posts ||..|{ post_logs

@enduml
```