


# 概述

定义

    别名： 依赖，发布/订阅（Another Name: Dependents, Publish/Subscribe）

作用

    当一个对象状态发生改变时，所有依赖它的对象都得到通知并被自动更新。

        当一个对象的数据更新时，需要通知其他对象，而又不希望和被通知的对象形成紧耦合时

作用

    解耦
    
    
    
# 示例    

## 类图  

接口关系图

![](https://github.com/RodJohn/DesignPattern/blob/master/image/observer.png)


## 完整代码 

传输消息体

    public enum WeatherEnum {
        SUNSHINE,
        RAIN;
    }

发布方

    public interface IWeather {
        WeatherEnum getInfo();
        void change();
    }


    public class Weather implements IWeather {

        private WeatherEnum info = WeatherEnum.SUNSHINE;

        @Override
        public WeatherEnum getInfo() {
            return info;
        }

        @Override
        public void change() {
            info = info == WeatherEnum.SUNSHINE ? WeatherEnum.RAIN : WeatherEnum.SUNSHINE;
        }
    }

订阅方

    public interface IPerson {
        void responseToWeather(WeatherEnum weather);
    }


    public class Person implements IPerson {
        @Override
        public void responseToWeather(WeatherEnum weather) {
            switch (weather){
                case RAIN : {
                    System.out.println(" It is to rain , I should buy a umbllera");
                    break;
                }
                case SUNSHINE: {
                    System.out.println(" It is to sunshine , I should buy a fan");
                    break;
                }
            }
        }
    }


订阅发布服务

    public interface IWeatherService {
        public IWeather weather();
        void addPerson(IPerson person);
        void removePerson(IPerson person);
        void broadcast();
    }

    public class WeatherService implements IWeatherService {

        private IWeather weather ;

        private List<IPerson> psersons  ;

        WeatherService(IWeather weather){
            this.weather = weather;
            psersons = new ArrayList<IPerson>();
        }
        
        @Override
        public IWeather weather() {
            return weather;
        }


        @Override
        public void addPerson(IPerson person) {
            psersons.add(person);
        }

        @Override
        public void removePerson(IPerson person) {
            psersons.remove(person);
        }

        @Override
        public void broadcast() {
            for (IPerson person : psersons ) {
                person.responseToWeather(weather.getInfo());
            }
        }
    }


客户端代码

    public class Test {
        public static void main(String[] args) {
            IWeather weather = new Weather();
            IPerson person = new Person();
            IWeatherService weatherService = new WeatherService(weather);
            weatherService.addPerson(person);

            weather.change();
            weatherService.broadcast();

            weather.change();
            weatherService.broadcast();
        }
    }




