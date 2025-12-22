# 1. xml映射

## 1.1. select

## 1.2. insert, update 和 delete

### 1.2.1. 批量插入数据

使用foreach标签动态sql拼接,完成一次性插入. 如果你的数据库支持自动生成主键的字段（比如 MySQL 和 SQL Server），那么你可以设置 useGeneratedKeys=”true”，然后再把 keyProperty 设置为目标属性就 OK 了

```
<insert id="insertAuthor" useGeneratedKeys="true"
    keyProperty="id">
  insert into Author (username, password, email, bio) values
  <foreach item="item" collection="list" separator=",">
    (#{item.username}, #{item.password}, #{item.email}, #{item.bio})
  </foreach>
</insert>
```

### 1.2.2. 批量更新数据

#### 1.2.2.1. 通过foreach循环

```
<update id="batchUpdate">
<foreach collection="list" item="item" index="index" open="" close="" separator=";">
    update re_exam
    <set>
        <if test="item.reExamState!=null ">
            re_state=#{item.reExamState},
        </if>
        <if test="item.reAddr!=null and item.reAddr!=''">
            re_addr=#{item.reAddr},
        </if>
        <if test="item.reScoretime!=null">
            re_score_time=#{item.reScoretime},
        </if>
        <if test="item.reFinalscore!=null and item.reFinalscore!=''">
            re_finalscore=#{item.reFinalscore},
        </if>
        <if test="item.teacherNames!=null and item.teacherNames!=''">
            teacher_names=#{item.teacherNames},
        </if>
        <if test="item.editable!=null">
            editable=#{item.editable},
        </if>
        re_cno=#{item.courseId}
    </set>
    where re_cno=#{item.courseId} and re_sno=#{item.stuId}
</foreach>

</update>
```

  

## 1.3. SQL 代码片段

## 1.4. 结果集映射