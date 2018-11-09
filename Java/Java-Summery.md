# Java-Summery

## 二、日期时间

### 1.java时间类型

`java.util.date` 除了SQL语句的情况下使用

`java.sql.date` 	针对SQL语句使用,表示日期，只有年月日，没有时分秒，**会丢失时间**

`java.sql.Time` 表示时间，只有时分秒，没有年月日，**会丢失日期**

`java.sql.Timestamp`表示时间戳，有年月日 时分秒，以及毫秒



`继承关系`![date继承](/home/erek/code/notes/Java/images/date-01.png)



### 2.时间类型相互转换

（1）把数据库的3种时间类型赋给java.util.date，不用转换，因为把子类对象给父类的引用

（2）把java.util.Date转换成数据库的三种类型

```java
java.utl.Date d = new java.util.Date();

java.sql.Date date = new java.sql.Date(d.getTime());　　//会丢失时分秒

Time time = new Time(d.getTime());　　　　　　　　　　  //会丢失年月日

Timestamp timestamp = new Timestamp(d.getTime());
```

### 3.关于时间的方法

日期和时间模式**注意大小写**

```
yyyy：年
MM：月
dd：日
hh：1~12小时制(1-12)
HH：24小时制(0-23)
mm：分
ss：秒
S：毫秒
E：星期几
D：一年中的第几天
F：一月中的第几个星期(会把这个月总共过的天数除以7)
w：一年中的第几个星期
W：一月中的第几星期(会根据实际情况来算)
a：上下午标识
k：和HH差不多，表示一天24小时制(1-24)。
K：和hh差不多，表示一天12小时制(0-11)。
z：表示时区
```

```java
package com.common;


import java.sql.Timestamp;
import java.text.DateFormat;
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.ArrayList;
import java.util.Calendar;
import java.util.Date;
import java.util.GregorianCalendar;
import java.util.HashMap;
import java.util.Iterator;
import java.util.LinkedHashMap;
import java.util.List;

/**
 * 日期工具类
 */
public class DateUtil{
	
	// ==格式到年== 
	/**
	 * 日期格式，年份，例如：2004，2008
	 */
	public static final String DATE_FORMAT_YYYY = "yyyy";
	
	
	// ==格式到年月 == 
	/**
	 * 日期格式，年份和月份，例如：200707，200808
	 */
	public static final String DATE_FORMAT_YYYYMM = "yyyyMM";

	/**
	 * 日期格式，年份和月份，例如：200707，2008-08
	 */
	public static final String DATE_FORMAT_YYYY_MM = "yyyy-MM";

	
	// ==格式到年月日== 
	/**
	 * 日期格式，年月日，例如：050630，080808
	 */
	public static final String DATE_FORMAT_YYMMDD = "yyMMdd";

	/**
	 * 日期格式，年月日，用横杠分开，例如：06-12-25，08-08-08
	 */
	public static final String DATE_FORMAT_YY_MM_DD = "yy-MM-dd";

	/**
	 * 日期格式，年月日，例如：20050630，20080808
	 */
	public static final String DATE_FORMAT_YYYYMMDD = "yyyyMMdd";
	
	/**
	 * 日期格式，年月日，用横杠分开，例如：2006-12-25，2008-08-08
	 */
	public static final String DATE_FORMAT_YYYY_MM_DD = "yyyy-MM-dd";
	
	/**
	 * 日期格式，年月日，例如：2016.10.05
	 */
	public static final String DATE_FORMAT_POINTYYYYMMDD = "yyyy.MM.dd";
	
	/**
	 * 日期格式，年月日，例如：2016年10月05日
	 */
	public static final String DATE_TIME_FORMAT_YYYY年MM月DD日 = "yyyy年MM月dd日";
	
	
	// ==格式到年月日 时分 == 
	
	/**
	 * 日期格式，年月日时分，例如：200506301210，200808081210
	 */
	public static final String DATE_FORMAT_YYYYMMDDHHmm = "yyyyMMddHHmm";

	/**
	 * 日期格式，年月日时分，例如：20001230 12:00，20080808 20:08
	 */
	public static final String DATE_TIME_FORMAT_YYYYMMDD_HH_MI = "yyyyMMdd HH:mm";
	
	/**
	 * 日期格式，年月日时分，例如：2000-12-30 12:00，2008-08-08 20:08
	 */
	public static final String DATE_TIME_FORMAT_YYYY_MM_DD_HH_MI = "yyyy-MM-dd HH:mm";
	
	
	// ==格式到年月日 时分秒== 
	/**
	 * 日期格式，年月日时分秒，例如：20001230120000，20080808200808
	 */
	public static final String DATE_TIME_FORMAT_YYYYMMDDHHMISS = "yyyyMMddHHmmss";
	
	/**
	 * 日期格式，年月日时分秒，年月日用横杠分开，时分秒用冒号分开
	 * 例如：2005-05-10 23：20：00，2008-08-08 20:08:08
	 */
	public static final String DATE_TIME_FORMAT_YYYY_MM_DD_HH_MI_SS = "yyyy-MM-dd HH:mm:ss";

	
	// ==格式到年月日 时分秒 毫秒== 
	/**
	 * 日期格式，年月日时分秒毫秒，例如：20001230120000123，20080808200808456
	 */
	public static final String DATE_TIME_FORMAT_YYYYMMDDHHMISSSSS = "yyyyMMddHHmmssSSS";
    
	
	// ==特殊格式==
	/**
	 * 日期格式，月日时分，例如：10-05 12:00
	 */
	public static final String DATE_FORMAT_MMDDHHMI = "MM-dd HH:mm";

	
	/* ************工具方法***************   */
	
	/** 
     * 获取某日期的年份
     * @param date 
     * @return 
     */
	public static Integer getYear(Date date) {
   	 	Calendar cal = Calendar.getInstance();
   	 	cal.setTime(date);
		return cal.get(Calendar.YEAR);
	}  
	
	/**
	 * 获取某日期的月份
	 * @param date
	 * @return
	 */
	public static Integer getMonth(Date date) {
		Calendar cal = Calendar.getInstance();
   	 	cal.setTime(date);
		return cal.get(Calendar.MONTH) + 1;
	}
	
	/**
	 * 获取某日期的日数
	 * @param date
	 * @return
	 */
	public static Integer getDay(Date date){
		Calendar cal = Calendar.getInstance();
		cal.setTime(date);
		 int day=cal.get(Calendar.DATE);//获取日
		 return day;
	}
	
    /**
     * 格式化Date时间
     * @param time Date类型时间
     * @param timeFromat String类型格式
     * @return 格式化后的字符串
     */
    public static String parseDateToStr(Date time, String timeFromat){
    	DateFormat dateFormat=new SimpleDateFormat(timeFromat);
    	return dateFormat.format(time);
    }
    
    /**
     * 格式化Timestamp时间
     * @param timestamp Timestamp类型时间
     * @param timeFromat
     * @return 格式化后的字符串
     */
    public static String parseTimestampToStr(Timestamp timestamp,String timeFromat){
    	SimpleDateFormat df = new SimpleDateFormat(timeFromat);
    	return df.format(timestamp);
    }
    
    /**
     * 格式化Date时间
     * @param time Date类型时间
     * @param timeFromat String类型格式
     * @param defaultValue 默认值为当前时间Date
     * @return 格式化后的字符串
     */
    public static String parseDateToStr(Date time, String timeFromat, final Date defaultValue){
    	try{
    		DateFormat dateFormat=new SimpleDateFormat(timeFromat);
        	return dateFormat.format(time);
    	}catch (Exception e){
    		if(defaultValue!=null)
				return parseDateToStr(defaultValue, timeFromat);
			else
				return parseDateToStr(new Date(), timeFromat);
    	}
    }
    
    /**
     * 格式化Date时间
     * @param time Date类型时间
     * @param timeFromat String类型格式
     * @param defaultValue 默认时间值String类型
     * @return 格式化后的字符串
     */
    public static String parseDateToStr(Date time, String timeFromat, final String defaultValue){
    	try{
    		DateFormat dateFormat=new SimpleDateFormat(timeFromat);
        	return dateFormat.format(time);
    	}catch (Exception e){
    		return defaultValue;
    	}
    }
    
    /**
     * 格式化String时间
     * @param time String类型时间
     * @param timeFromat String类型格式
     * @return 格式化后的Date日期
     */
    public static Date parseStrToDate(String time, String timeFromat) {
    	if (time == null || time.equals("")) {
    		return null;
    	}
    	
    	Date date=null;
    	try{
	    	DateFormat dateFormat=new SimpleDateFormat(timeFromat);
	    	date=dateFormat.parse(time);
    	}catch(Exception e){
    		
    	}
    	return date;
    }
    
    /**
     * 格式化String时间
     * @param strTime String类型时间
     * @param timeFromat String类型格式
     * @param defaultValue 异常时返回的默认值
     * @return
     */
    public static Date parseStrToDate(String strTime, String timeFromat,
			Date defaultValue) {
		try {
			DateFormat dateFormat = new SimpleDateFormat(timeFromat);
			return dateFormat.parse(strTime);
		} catch (Exception e) {
			return defaultValue;
		}
	}
    
    /**
     * 当strTime为2008-9时返回为2008-9-1 00:00格式日期时间，无法转换返回null.
     * @param strTime
     * @return
     */
    public static Date strToDate(String strTime) {
    	if(strTime==null || strTime.trim().length()<=0)
    		return null;
    	
		Date date = null;
		List<String> list = new ArrayList<String>(0);
		
		list.add(DATE_TIME_FORMAT_YYYY_MM_DD_HH_MI_SS);
		list.add(DATE_TIME_FORMAT_YYYYMMDDHHMISSSSS);
		list.add(DATE_TIME_FORMAT_YYYY_MM_DD_HH_MI);
		list.add(DATE_TIME_FORMAT_YYYYMMDD_HH_MI);
		list.add(DATE_TIME_FORMAT_YYYYMMDDHHMISS);
		list.add(DATE_FORMAT_YYYY_MM_DD);
		//list.add(DATE_FORMAT_YY_MM_DD);
		list.add(DATE_FORMAT_YYYYMMDD);
		list.add(DATE_FORMAT_YYYY_MM);
		list.add(DATE_FORMAT_YYYYMM);
		list.add(DATE_FORMAT_YYYY);
		
		
		for (Iterator iter = list.iterator(); iter.hasNext();) {
			String format = (String) iter.next();
			if(strTime.indexOf("-")>0 && format.indexOf("-")<0)
				continue;
			if(strTime.indexOf("-")<0 && format.indexOf("-")>0)
				continue;
			if(strTime.length()>format.length())
				continue;
			date = parseStrToDate(strTime, format);
			if (date != null)
				break;
		}

		return date;
	}
    
    /**
	 * 解析两个日期之间的所有月份
	 * @param beginDateStr 开始日期，至少精确到yyyy-MM
	 * @param endDateStr 结束日期，至少精确到yyyy-MM
	 * @return yyyy-MM日期集合
	 */  
    public static List<String> getMonthListOfDate(String beginDateStr, String endDateStr) {  
        // 指定要解析的时间格式  
        SimpleDateFormat f = new SimpleDateFormat("yyyy-MM");  
        // 返回的月份列表  
        String sRet = "";  
  
        // 定义一些变量  
        Date beginDate = null;  
        Date endDate = null;  
  
        GregorianCalendar beginGC = null;  
        GregorianCalendar endGC = null;  
        List<String> list = new ArrayList<String>();  
  
        try {  
            // 将字符串parse成日期  
            beginDate = f.parse(beginDateStr);  
            endDate = f.parse(endDateStr);  
  
            // 设置日历  
            beginGC = new GregorianCalendar();  
            beginGC.setTime(beginDate);  
  
            endGC = new GregorianCalendar();  
            endGC.setTime(endDate);  
  
            // 直到两个时间相同  
            while (beginGC.getTime().compareTo(endGC.getTime()) <= 0) {  
                sRet = beginGC.get(Calendar.YEAR) + "-"  
                        + (beginGC.get(Calendar.MONTH) + 1);  
                list.add(sRet);  
                // 以月为单位，增加时间  
                beginGC.add(Calendar.MONTH, 1);  
            }  
            return list;  
        } catch (Exception e) {  
            e.printStackTrace();  
            return null;  
        }  
    }  
  
    /** 
     * 解析两个日期段之间的所有日期
     * @param beginDateStr 开始日期  ，至少精确到yyyy-MM-dd
     * @param endDateStr 结束日期  ，至少精确到yyyy-MM-dd
     * @return yyyy-MM-dd日期集合
     */  
    public static List<String> getDayListOfDate(String beginDateStr, String endDateStr) {  
        // 指定要解析的时间格式  
        SimpleDateFormat f = new SimpleDateFormat("yyyy-MM-dd");  
  
        // 定义一些变量  
        Date beginDate = null;  
        Date endDate = null;  
  
        Calendar beginGC = null;  
        Calendar endGC = null;  
        List<String> list = new ArrayList<String>();  
  
        try {  
            // 将字符串parse成日期  
            beginDate = f.parse(beginDateStr);  
            endDate = f.parse(endDateStr);  
  
            // 设置日历  
            beginGC = Calendar.getInstance();  
            beginGC.setTime(beginDate);  
  
            endGC = Calendar.getInstance();  
            endGC.setTime(endDate);  
            SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");  
  
            // 直到两个时间相同  
            while (beginGC.getTime().compareTo(endGC.getTime()) <= 0) {  
  
                list.add(sdf.format(beginGC.getTime()));  
                // 以日为单位，增加时间  
                beginGC.add(Calendar.DAY_OF_MONTH, 1);  
            }  
            return list;  
        } catch (Exception e) {  
            e.printStackTrace();  
            return null;  
        }  
    }  
}
```

## 一、小数

### 1.float double 区别

- float： 占4个字节，以32位表示，1个符号位，8位指数和23位有效数字

- double: 占8个字节，以64位表示，1个符号位，11位指数和52位有效数字

默认情况下，java用double表示浮点数字

### 2.关于小数的方法

#### (1)保留小数的方法

```java
// 1.用DecimalFormat类
// 保留两位小数
double d = 0.200;
DecimalFormat df = new DecimalFormat("0.00");
System.out.println(df.format(d));

// 2.
double d = 0.6544;
String s = String.format("%.2f",d);
System.out.println(s);

// 3.使用BigDecimal类

double d = 1.000;
BigDecimal bd=new BigDecimal(d);
double d1=bd.setScale(2,BigDecimal.ROUND_HALF_UP).doubleValue();
System.out.println(d1);

```





