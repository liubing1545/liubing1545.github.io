1. checkout

   ```shell
   git checkout --orphan latest_branch
   ```

2. 添加所有文件

   ```shell
   git add -A
   ```
   ​

3. 提交变更

   ```shell
   git commit -am "something about message"
   ```
   ​

4. 删除分支

   ```shell
   git branch -D master
   ```
   ​

5. 重命名当前branch为目标名

   ```shell
   git branch -m master
   ```

   ​
6. 强制push

   ```shell
   git push -f origin master
   ```
   ​