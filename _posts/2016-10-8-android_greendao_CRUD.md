---
layout:     post
title:      "Android_greenDao增删改查"
subtitle:   "greenDao是一个使用于android的ORM框架"
date:       2016-10-8
tags:
    - android
    - sql
---

## 简介

`greenDao`是一个使用于`android`的`ORM`框架,
现在主流的`ORM`框架有`OrmLite`,`SugarORM`,`Active Android`,`Realm`以及`GreenDAO`.`greenDao`的性能远远高于同类的`ORM`框架,具体的测试结果官网有。


## 示例代码
[https://github.com/7449/AndroidDevelop/blob/develop/GreenDao](https://github.com/7449/AndroidDevelop/blob/develop/GreenDao/)


	有时候数据表会由后台分发而不是自动生成，所以这里简单介绍下怎么使用greendao操作这种情况下的数据表

	需要注意以下几点
		
		1：android数据库操作都是在databases文件夹下，所以要把需要动的数据库复制到databases文件夹下

	 	/**
	     * assets目录下的db转移到databases
	     */
	    public void copyDBToDatabases() {
	        try {
	            String outFileName = DB_PATH + DB_NAME;
	            File file = new File(DB_PATH);
	            if (!file.mkdirs()) {
	                file.mkdirs();
	            }
	            File dataFile = new File(outFileName);
	            if (dataFile.exists()) {
	                dataFile.delete();
	            }
	            InputStream myInput;
	            myInput = getApplicationContext().getAssets().open(DB_NAME);
	            OutputStream myOutput = new FileOutputStream(outFileName);
	            byte[] buffer = new byte[1024];
	            int length;
	            while ((length = myInput.read(buffer)) > 0) {
	                myOutput.write(buffer, 0, length);
	            }
	            myOutput.flush();
	            myOutput.close();
	            myInput.close();
	        } catch (IOException e) {
	            Log.i(TAG, "error--->" + e.toString());
	            e.printStackTrace();
	        }
	
	    }


		2：geenDao自动生成的时候table 默认为TABLENAME，默认id生成时"_id",这些生成的字段必须要和数据表中的字段一致，可以用 nameInDb="" 指定字段name和tableName

	
		@Entity(nameInDb = "blacklist")
		public class ExternalBean {
		    @Property(nameInDb = "id")
		    private Integer id;
		    @Property(nameInDb = "email")
		    private String email;
		    @Generated(hash = 1314000312)
		    public ExternalBean(Integer id, String email) {
		        this.id = id;
		        this.email = email;
		    }
		    @Generated(hash = 981826822)
		    public ExternalBean() {
		    }
		    public Integer getId() {
		        return this.id;
		    }
		    public void setId(Integer id) {
		        this.id = id;
		    }
		    public String getEmail() {
		        return this.email;
		    }
		    public void setEmail(String email) {
		        this.email = email;
		    }
		}


## 3.x更新


项目`build.gradle`添加

	classpath 'org.greenrobot:greendao-gradle-plugin:3.2.0'

`app`下的`build.gradle`添加

	
	apply plugin: 'org.greenrobot.greendao'

	compile 'org.greenrobot:greendao:3.2.0'

	    //修改生成类的位置
	    greendao {
	        targetGenDir 'src/main/java/' //生成源文件的目录，默认是build目录中的(build/generated/source/greendao)
	//        daoPackage //生成的DAO，DaoMaster和DaoSession的包名。默认是实体的包名
	//        schemaVersion //当前数据库结构的版本
	//        generateTests //设置是否自动生成单元测
	//        targetGenDirTest //生成的单元测试的根目录
	    }

然后自定义`UserBean`类

	@Entity
	public class UserBean {
	
	    @Id
	    private Long id;
	    private String name;
	    private int age;
	    @Generated(hash = 1032023074)
	    public UserBean(Long id, String name, int age) {
	        this.id = id;
	        this.name = name;
	        this.age = age;
	    }
	    @Generated(hash = 1203313951)
	    public UserBean() {
	    }
	}

注解：

	@Entity 定义实体
	@nameInDb 在数据库中的名字，如不写则为实体中类名
	@indexes 索引
	@createInDb 是否创建表，默认为true,false时不创建
	@schema 指定架构名称为实体
	@active 无论是更新生成都刷新
	@Id
	@NotNull 不为null
	@Unique 唯一约束
	@ToMany 一对多
	@OrderBy 排序
	@ToOne 一对一
	@Transient 不存储在数据库中
	@generated 由greendao产生的构造函数或方法


重新`rebuild` 一下项目，就可以看到`greenDao`自动生成的数据库相关类，比2.X时确实好用多了，之后用法还和以前一样

## 2.x使用

>用greenDao实现了数据库的增删改查,确实比以前自己写SQL语句舒服多了,不用再考虑SQL语句很方便。


IDE工具：AndroidStudio

#### gradle引用

    compile 'org.greenrobot:greendao-generator:2.2.0'
    compile 'org.greenrobot:greendao:2.2.0'

#### Generator

单独创建一个目录，起名为`sql`,然后创建一个`GreenDaoGenerator`,注意一点是这里就要运行一下这个`java`类，不是`app`是这个`java`类，需要单独运行一下生成`sql`代码

	public class MyGreenDaoGenerator {

	    public static void main(String[] args) throws Exception {
	        //版本号,包名
	        Schema schema = new Schema(1, "github.com.greendao.sql");
	        Entity user = schema.addEntity("User");
	        user.addIdProperty().primaryKey();
	        user.addStringProperty("userName").notNull();
	        user.addStringProperty("userSex").notNull();
	        new DaoGenerator().generateAll(schema, "./app/src/main/java");
	    }
	
	}

最后会自动生成这几个文件

![_config.yml]({{ site.baseurl }}/assets/screenshot/16/greendao.png)


#### 增删改查


	public class MainActivity extends AppCompatActivity implements View.OnClickListener {
	
	    private EditText userName;
	    private EditText userSex;
	    private EditText etDelete;
	    private EditText etUpDateId;
	    private EditText etUpDateName;
	    private EditText etUpDateSex;
	    private EditText etSearch;
	
	
	    private SQLiteDatabase writableDatabase;
	    private UserDao userDao;
	    private ListView listView;
	    private SimpleCursorAdapter simpleCursorAdapter;
	
	
	    @Override
	    protected void onCreate(Bundle savedInstanceState) {
	        super.onCreate(savedInstanceState);
	        setContentView(R.layout.activity_main);
	
	        userName = (EditText) findViewById(R.id.userName);
	        userSex = (EditText) findViewById(R.id.userSex);
	        listView = (ListView) findViewById(R.id.list);
	        etDelete = (EditText) findViewById(R.id.et_delete);
	        etUpDateId = (EditText) findViewById(R.id.et_update_id);
	        etUpDateName = (EditText) findViewById(R.id.et_update_name);
	        etUpDateSex = (EditText) findViewById(R.id.et_update_sex);
	        etSearch = (EditText) findViewById(R.id.et_search);
	
	        findViewById(R.id.add).setOnClickListener(this);
	        findViewById(R.id.delete).setOnClickListener(this);
	        findViewById(R.id.update).setOnClickListener(this);
	        findViewById(R.id.search).setOnClickListener(this);
	
	        init();
	    }
	
	    private void init() {
	        writableDatabase = new DaoMaster.DevOpenHelper(this, "greendao", null).getWritableDatabase();
	        DaoSession daoSession = new DaoMaster(writableDatabase).newSession();
	
	        //得到Dao的对象
	        userDao = daoSession.getUserDao();
	        // 遍历表中所有的数据
	
	        Cursor cursor = writableDatabase.query(userDao.getTablename(), userDao.getAllColumns(), null, null, null, null, null);
	        String[] from = {UserDao.Properties.UserName.columnName, UserDao.Properties.UserSex.columnName};
	        int[] id = {android.R.id.text1, android.R.id.text2};
	        simpleCursorAdapter = new SimpleCursorAdapter(this, android.R.layout.simple_list_item_2, cursor, from, id, Adapter.NO_SELECTION);
	        listView.setAdapter(simpleCursorAdapter);
	    }
	
	    @Override
	    public void onClick(View v) {
	        switch (v.getId()) {
	            case R.id.add:
	                if (!userName.getText().toString().isEmpty() && !userSex.getText().toString().isEmpty()) {
	                    addSQLite(userName.getText().toString(), userSex.getText().toString());
	                    userName.getText().clear();
	                    userSex.getText().clear();
	                } else {
	                    Toast.makeText(getApplicationContext(), "name sex must not null", Toast.LENGTH_LONG).show();
	                }
	                break;
	
	            case R.id.delete:
	                if (!etDelete.getText().toString().isEmpty()) {
	                    deleteSQLite(Long.valueOf(etDelete.getText().toString().trim()));
	                    etDelete.getText().clear();
	                } else {
	                    Toast.makeText(getApplicationContext(), "id illegal", Toast.LENGTH_LONG).show();
	                }
	                break;
	
	            case R.id.update:
	                if (!etUpDateId.getText().toString().isEmpty() && !etUpDateName.getText().toString().isEmpty() && !etUpDateSex.getText().toString().isEmpty()) {
	                    upDateSQLite(Long.parseLong(etUpDateId.getText().toString().trim()), etUpDateName.getText().toString().trim(), etUpDateSex.getText().toString().trim());
	                    etUpDateId.getText().clear();
	                    etUpDateName.getText().clear();
	                    etUpDateSex.getText().clear();
	                } else {
	                    Toast.makeText(getApplicationContext(), "id name sex must not null", Toast.LENGTH_LONG).show();
	                }
	                break;
	
	            case R.id.search:
	                if (!etSearch.getText().toString().isEmpty()) {
	                    searchSQLite(Long.parseLong(etSearch.getText().toString().trim()));
	                    etSearch.getText().clear();
	                } else {
	                    Toast.makeText(getApplicationContext(), "id illegal", Toast.LENGTH_LONG).show();
	                }
	                break;
	
	        }
	        Cursor cursor = writableDatabase.query(userDao.getTablename(), userDao.getAllColumns(), null, null, null, null, null);
	        simpleCursorAdapter.swapCursor(cursor);
	    }
	
	    //add sql data
	    private void addSQLite(String name, String sex) {
	
	        User user = new User(null, name, sex);
	
	        userDao.insert(user);
	    }
	
	
	    //delete sql data
	    private void deleteSQLite(Long id) {
	
	        userDao.deleteByKey(id);
	        //delete sql all;
	//        userDao.deleteAll();
	
	    }
	
	
	    //update sql data
	    private void upDateSQLite(long id, String name, String sex) {
	        userDao.update(new User(id, name, sex));
	    }
	
	    //search sql data
	    private void searchSQLite(long id) {
	        QueryBuilder<User> queryBuilder = userDao.queryBuilder().where(UserDao.Properties.Id.eq(id));
	        // .list() Returns a collection of entity classes
	        List<User> user = queryBuilder.list();
	        // If you only want results , use .unique() method
	        // Person person = queryBuilder.unique();
	        new AlertDialog
	                .Builder(this)
	                .setMessage(user != null && user.size() > 0 ? user.get(0).getUserName() + "--" + user.get(0).getUserSex() : "data null")
	                .setPositiveButton("ok", null).create().show();
	    }
	}



![_config.yml]({{ site.baseurl }}/assets/screenshot/16/greendao_image.gif)

