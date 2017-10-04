## primitive ##
- Byte
- Short
- Int
- Long
- Float
- Double


## var、val ##
    fun main(args: Array<String>) {
        var a = 1 //声明变量，自动推断为Int数据类型
        val b = "Kotlin" //声明常量，不可再重新初始化
        println(a.javaClass)
        println(b.javaClass)
    }

## main、fun、params ##
    //main方法，无返回值为Unit可不写
    fun main(args: Array<String>) {
        println(plus1(1, "Hello"))
        println(plus2(a = 1, b = "Hello")) //具名参数，给参数的实参附上形参
        println(plus3(b = "Hello")) //可变参数，即多参数，传参有歧义时需要使用具名参数
    }
    
    //函数写法1
    fun plus1(a: Int, b: String): String {
        return a.toString() + b
    }
    
    //函数写法2（只有一行时，推荐）
    fun plus2(a: Int, b: String) = a.toString() + b
    
    //a: Int = 1为默认参数
    fun plus3(a: Int = 1, b: String) = a.toString() + b

## 函数式表达式 ##
    fun main(args: Array<String>) {
        println(add(1, 2))
    
        // var 不仅可以声明变量，也可以声明函数
        var i = { x: Int, y: Int -> x + y } //写法1，推荐
        println(i(1, 2))
    
        var j: (Int, Int) -> Int = { x, y -> x + y }//写法2
        println(j(1, 2))
    }
    
    fun add(x: Int, y: Int) = x + y
    

## if、else ##
    fun checkAge(age: Int) = if (age > 18) "成年人" else "未成年人"

## $ 、readLine() 、!!  ##
    fun main(args: Array<String>) {
        println("请输入第一个数")
        var a = readLine() //键盘录入
        println("请输入第二个数")
        var b = readLine()
        //${}字符串模版
        //!!非空判断
        println("${a}+" + "${b}=" + "${a!!.toInt() + b!!.toInt()}")
    }

## ==、equal ##
    //字符串比较
    fun main(args: Array<String>) {
        var str1 = "kotlin"
        var str2 = "Kotlin"
        println(str1 == str2)
        println(str1.equals(str2))
        println(str1.equals(str2, true)) //第二个参数为true代表忽略大小写
    }

## ? null ##
    fun main(args: Array<String>) {
      nullCheck(null)
    }

    //Kotlin 函数默认接收是非空对象，?显示声明可接受null对象
    fun nullCheck(str: String?) = str

## when ##
    fun main(args: Array<String>) {
        println(switchGrade(12))
    }

    //Java switch的加强版，lambda表达式，有返回值
    fun switchGrade(age: Int): String {
        return when (age) {
            7 -> "一年级"
            8 -> "二年级"
            9 -> "三年级"
            10 -> "四年级"
            11 -> "五年级"
            12 -> "六年级"
            else -> "不是小学生"
        }
    }
    
## loop、Range ##
       fun main(args: Array<String>) {
           var nums1 = 1..100 //[1,100]
           println(nums1.javaClass) //IntRange
           for (i in nums1 ){
               println(i)
           }
           
           var nums2 = 1 until 100 //[1,100)100开区间
           var nums3 = nums2.reversed() //反转
           for (i in nums3 step 2){ //loop循环, step每次i跳过几步
               println(i)
           }
       }

## list ##
    fun main(args: Array<String>) {
        var lists = listOf("Java", "Kotlin", "C#", "Phyon")
        for (list in lists) {
            println(list)
        }
        for (list in lists.withIndex()) {
            println("${list.index}    ${list.value}")// list javaClass -> IndexedValue
        }
        for ((i, v) in lists.withIndex()) {
            println("$i    $v")
        }
    }

## map ##
    fun main(args: Array<String>) {
        var map = mapOf(Pair(1,"Kotlin"), Pair(2,"Java"), Pair(3,"C#"))
        for ((k,v) in map){
            println("$k     $v")
        }
    
        var  treeMap = TreeMap<String, String>()
        treeMap["Kobe"] = "科比"
        treeMap["Yao"] = "姚明"
        treeMap["Curry"] = "库里"
        
        println(treeMap["Kobe"])
    }
    
## tailrec ##
    fun main(args: Array<String>) {
    //    println(ollAdd(10000)) // StackOverflowError
        println(ollAddTailrec(1000000, 0))
    }
    
    //累加操作 5->5+4+3+2+1
    fun ollAdd(num: Int): Int { 
        println("第 ${num}  次计算")
        if (num == 1) return 1
        else return num + ollAdd(num - 1)
    }
    
    //tailrec尾递归优化
    tailrec fun ollAddTailrec(num: Int, result: Int): Int {
        println("第 ${num}  次计算, result= ${result}")
        if (num == 0) return 1
        else return ollAddTailrec(num - 1, result + num)
    }
    
## class 、data class ##
    fun main(args: Array<String>) {
        var kobe = Person("Kobe", "男", 38, "打篮球")
        println(kobe.toString())
        kobe.smile()
        println(kobe.selfIntroduction())
    
        var kobe1 = Person1("Kobe", "男", 38)
        println(kobe1.name + "  " + kobe1.sex + "  " + kobe1.age)
    
        var kobe2 = Person2("Kobe", "男", 38)
        println(kobe2.toString())
        println(kobe2.name + "  " + kobe2.sex + "  " + kobe2.age + "  " + kobe2.favorite)
    }
    
    class Person(name: String, sex: String, age: Int, favorite: String = "吃饭睡觉") {
    
        var name = name
        var age = age
        var favorite = favorite
    
        fun smile() {
            println("哈哈")
        }
    
        fun selfIntroduction(): String {
            return "我叫${name} ，今年${age}岁，爱好是${favorite}"
        }
    
        override fun toString(): String {
            return "Person(name='$name', age=$age, favorite='$favorite')"
        }
    }
    
    class Person1 {
        lateinit var name: String // lateinit延迟初始化
        lateinit var sex: String
        var age: Int = 0
    
        //构造函数重载，
        constructor(name: String, sex: String)
    
        constructor(name: String, sex: String, age: Int) {
            this.name = name
            this.sex = sex
            this.age = age
        }
    
        constructor(name: String, sex: String, age: Int, favorite: String = "吃饭睡觉")
    
    }
    
    //相当于JavaBean,已自动set/get/toString()
    data class Person2(val name: String, val sex: String, val age: Int, val favorite: String = "吃饭睡觉")
    
## override、: ##
    fun main(args: Array<String>) {
        var pixel = Pixel()
        println("${pixel.cpu}    ${pixel.call()}")
    }
    
    open class AndroidPhone{
        var cpu = 32
        open fun call() =  "打电话"
    }
    
    class Pixel :AndroidPhone(){    //:继承
        override fun call() = "摇一摇打电话" //重载
    }
    
## abstract ##
    fun main(args: Array<String>) {
        ChineseDog("中华田园犬").bark()
        TibetanDog("藏獒").bark()
    }
    
    abstract class Dog(name:String){
        abstract fun bark()
    }
    
    class ChineseDog(val name: String) :Dog(name){
        override fun bark() {
            println("${name}旺旺旺的叫")
        }
    }
    
    class TibetanDog(val name: String) :Dog(name){
        override fun bark() {
            println("${name}嗷嗷嗷的叫")
        }
    }
    
##  interface ##
    fun main(args: Array<String>) {
       var lists = listOf(Man(),TaiJian())//多态
        for (human in lists) {
            human.eat()
            if (human is IMan) human.xiaodidi()
        }
    }
    
    abstract class Human(){
        abstract fun eat()
    }
    
    interface IMan {
        fun xiaodidi()
    }
    
    class Man :Human(),IMan{
        override fun xiaodidi() {
            println("我有18cm的小弟弟")
        }
    
        override fun eat() {
            println("自己做饭吃")
        }
    }
    
    class TaiJian :Human(){
        override fun eat() {
            println("吃宫里的")
        }
    }

## by、object ##
    fun main(args: Array<String>) {
        var xiaoHuang = Human()
        xiaoHuang.buyIphoneX()
    }
    
    interface BuyPhone{
        fun buyIphoneX()
    }
    
    //委托给Cattle
    class Human:BuyPhone by Cattle(){
        override fun buyIphoneX() {
            println("找黄牛预约，不用去通宵排队")
            Cattle().buyIphoneX()
            println("开心的交钱拿到手机了")
        }
    }
    
    //代理了Human
    //object 单例，确保预约和交钱的是同一个黄牛
    object Cattle :BuyPhone{ 
        override fun buyIphoneX() {
            println("通宵蹲点，买苹果手机")
        }
    }


## companion object ##
    //companion object是在类加载的时候初始化的，相当于java中的静态内部类（kotlin没有static关键字）
    companion object {
            //伴生对象里可存放常量、方法
            val GITHUB_URL = "https://github.com/KobeBryant824"
            fun sayHello(){}
        }


## enum ##
    fun main(args: Array<String>) {
        println("${Direction.TOP} ${Direction.TOP.ordinal}")
    }
    
    //更在意数据
    enum class Direction {
        TOP, BOTTOM, LEFT, RIGHT
    }
    
    
## sealed ##
    fun main(args: Array<String>) {
       var brother1 = CurryBrothers.Klay()
       var brother2 = CurryBrothers.Kevin()
       var brother3 = CurryBrothers.Green()
    
        var lists = listOf(brother1, brother2, brother3)
        for (list in lists){
            (list as? CurryBrothers)?.noBrothernoBasketball()
        }
    }
    
    //印章类、密封类,用于限定子类类型，更在意数据类型
    sealed class CurryBrothers{
    
        fun noBrothernoBasketball(){
            println("无兄弟不篮球")
        }
    
        class Klay:CurryBrothers()
        class Kevin:CurryBrothers()
        class Green:CurryBrothers()
    }
    
    
## lateinit、lazy ##
    private lateinit var name？: String //lateinit懒加载、延迟初始化，只能修饰var，不能修饰可空的属性、基本数据类型
    private val sex: String by lazy {
            "男" //lazy{}可以延迟到一定实际再使用并初始化的final变量，这Java中是做不到的，只能用在val
        }


## inline ##
    // 内联函数与一般的函数不同，在编译时会做替换，少了普通函数调用的压栈出栈，更高效。因为是替换，所以可以识别传入的泛型
    inline fun <reified T> T.debug123(log: Any){
        Log.d(T::class.simpleName, log.toString())
    }

## 扩展函数 ##
    //Context:扩展函数的所属对象
    //. :扩展函数修饰符
    //toast：扩展函数的名称
    //在有Context地方直接调用 toast("kotlin")
    fun Context.toast(message: String, duration: Int = Toast.LENGTH_SHORT) {
        Toast.makeText(this, message, duration).show()
    }

## map ##
    fun main(args: Array<String>) {
        var map = mapOf(Pair(1, "a"), Pair(2, "b"))
        //lambda表达式:当Java函数的形参是个接口且只有一个抽象方法可以用lambda代替
        map.map { it -> //map接收的是一个lambda表达式,用于遍历容器中的每个数据
            println(it.key)
            println(it.value)
        }
        map.map { println(it) } //当方法体只有一行时可简写
    }

## flatMap ##
    fun main(args: Array<String>) {
        var lists = listOf(1, 2, 3, 4, 5, 6, 7)
        /**
         * 1 -> 1,2
         * 2 -> 2,3
         * 3 -> 3,4
         * 4 -> 4,5
         * 5 -> 5,6
         * 6 -> 6,7
         * 7 -> 7,8
         *
         */
        //flapMap 用于将原Iterable<T>转换成另一种Iterable<R>
        var flatMap = lists.flatMap { listOf(it, it + 1) }
        println(flatMap)
        flatMap.map { println(it) }
    }

