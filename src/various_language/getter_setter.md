# 类的属性的`getter`和`setter`

之前接触计算机语言，比如`C`、`C++`等，都没有`getter`和`setter`的

后来接触到了一些语言：
* `C#`
* `Swift`
* `Python`

其**对象**==**类**==**Class**中都有了**属性Property**的：
* `getter`：读取属性的值
* `setter`：设置（写入）属性的值

比如：

## Python
Python的[python - 新手学习Flask引用flask-login后登入错误原因不清 - SegmentFault](https://segmentfault.com/q/1010000002881544)
中的：
```python
class Admin(UserMixin, db.Model):
    __tablename__ = 'admin'
    id = db.Column(db.Integer, primary_key=True)
    username = db.Column(db.String(64), unique=True)
    password_hash = db.Column(db.String(128), unique=False)

    @property
    def password(self):
        raise AttributeError('不能直接获取明文密码！')

    @password.setter
    def password(self, password):
        self.password_hash = generate_password_hash(password)
```
