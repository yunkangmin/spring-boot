# 정리
1. 자바의 @Mapper가 있고 마이바티스에 @Mapper가 있다. 여기서는 자바의 @Mapper를 말한다.  
  
오늘 다루어볼 내용은 Model Mapping을 아주 쉽게 해주는 Mapstruct라는 라이브러리를  
다루어볼 것이다. 그 전에 Model mapping에 많이 사용되는 ModelMapper와의 간단한 차이를  
이야기해보면, Model Mapper는 Runtime 시점에 reflection으로 모델 매핑을 하게 되는데  
이렇게 런타임시에 리플렉션이 자주 이루어진다면 앱 성능에 아주 좋지 않다.  
하지만 Mapstruct는 컴파일 타임에 매핑 클래스를 생성해줘 그 구현체를 런타임에  
사용하는 것이기 때문에 앱 사이즈는 조금 커질 수 있지만 성능상 크게 이슈가 없어서  
ModelMapper보다 더 부담없이 사용하기 좋다. 그리고 아주 다양한 기능을 제공하기 때문에   
조금 더 세밀한 매핑이 가능해진다.   
  
바로 간단하게 Mapstruct를 맛보자.

# build.gradle
build.gradle를 채워넣어보자  
```
buildscript {
  ext {
    mapstructVersion = '1.3.1.Final'
  }
}

...

//Mapstruct
implementation 'org.mapstuct:mapstruct:${mapstructVersion}"
annotationProcessor "org.mapstruct:mapstruct-processor:${mapstructVersion}"
testAnnotationProcessor "org.mapstruct:mapstruct-processor:${mapstructVersion}"

...

compileJava {
  options.compilerArgs = {
    '-Amapstruct.suppressGeneratorTimestamp=true',
    '-Amapstruct.suppressGeneratorVersionInfoComment=true'
  }
}
```

# Java
```
@Data
@AllArgsConstructor(staticName = "of")
@NoArgsConstructor
public class CarDto {
  private String name;
  private String color;
}

@Data
@AllArgsConstructor(staticName = "of")
@NoArgsConstructor
public class Car {
  private String modelName;
  private String modelColor;
}

@Mapper
public interface CarMapper {
  CarMapper INSTANCE = Mappers.getMapper(CarMapper.class);
  
  //source가 파라미터로 온 CarDto
  //target이 반환될 Car
  @Mapping(source = "name", target = "modelName")
  @Mapping(source = "color", target = "modelColor")
  Car to(CarDto carDto);
}
```

사실 간단한 것이라, 어노테이션만 보아도 어떠한 기능을 제공하는지 확실하다.  
하지만 각 클래스에 getter/setter 및 기본 생성자는 꼭 생성해주자(필드명이  
동일할 일은 많이 없겠지만, 각 객체의 필드명이 동일하면 @Mapping 어노테이션은  
생략가능하다.)


```java
//@Mapper
public class MapstructTest {
  
  @Test
  public void test() {
    CarDto carDto = CarDto.of("bmw x4", "black");
    Car car = CarMapper.INSTANCE.to(carDto);
    
    assertEquals(carDto.getName(), car.getModelName());
    assertEquals(carDto.getColor(), car.getModelColor());
  }
}
```
테스트가 성공하는 것을 볼 수 있다. 아주 간단하면서도 파워풀한 라이브러리인 것 같다.  
그러면 실제로 생성된 코드를 한 번 보자.  
```
//@Mapper를 생성하면 자동으로 생성되는 코드
@Generated(
  value = "org.mapstruct.ap.MappingProcessor"
)
public class CarMapperImpl implements CarMapper {
  
  @Override
  public Car to(CarDto carDto) {
    if (carDto == null) {
      return null;
    }
    
    Car car = new Car();
    
    car.setModelName(carDto.getName());
    car.setModelColor(carDto.getColor());
    
    return car;
  }
}
```
우리가 작성하지 않았지만, 코드가 생성되었다. 리플렉션과 같이 비용이 큰 기술이  
사용되지 않았다. 간단한 validation 코드와 생성자, getter, setter 코드 뿐이다.  
이제 더 많은 Mapstruct 기능들을 살펴보자. 

# 하나의 객체로 합치기
여러 객체의 필드값을 하나의 객체로 합치기가 가능하다. 특별히 다른 옵션을  
넣는 것은 아니다.   
```
@Data
@AllArgsConstructor(staticName = "of")
@NoArgsConstructor
public class UserDto {
  private String name;
}

@Data
@AllArgsConstructor(staticName = "of")
@NoArgsConstructor
public class AddressDto {
  private String si;
  private String dong;
}

@Mapper
public interface UserInfoMapper {
  UserInfoMapper INSTANCE = Mappers.getMapper(UserInfoMapper.class);
  
  @Mapping(source = "user.name", target = "userName")
  @Mapping(source = "address.si", target = "si")
  @Mapping(source = "address.dong",target = "dong")
  UserInfo to(UserDto user, AddressDto address);
}

@Generated(
  value = "org.mapstruct.ap.MappingProcessor"
)
public class UserInfoMapperImpl implements UserInfoMapper {
  
  @Override
  public UserInfo to(UserDto user, AddressDto address) {
    if (user == null && address == null) {
      return null;
    }
    
    UserInfo userInfo = new UserInfo();
    
    if (user != null) {
      userInfo.setUserName(user.getName());
    }
    if (address != null) {
      userInfo.setDong(address.getDong());
      userInfo.setSi(address.getSi());
    }
    
    return userInfo;
  }
}
```

...

[참고](https://coding-start.tistory.com/349)

---

클래스간 변환을 간편하게 해주는 라이브러리이다(Car <> CarDTO).   
아래는 Mapper 선언법이다.  
```
@Mapper(componentModel = "spring", uses = {
  OwnerMapper.class //3
})
public interface CarMapper {  
  CarMapper INSTANCE = Mappers.getMapper(CarMapper.class);
  
  @Mapping(source = "numberOfSeats", target = "seatCount") //1
  CatDto toDto(Car car);
  
  @Mapping(target = "id", ignore = true) //2
  Car toEntity(CarDTO dto);
}
```
보다 시피 entity를 DTO로 변환해주는 작업을 한다.  
(지금은 엔티티와 DTO를 예시로 들었지만 어떤 클래스든 상관없다.)  
아래의 generate된 코드를 보면 알 수 있겠지만, 변환될 클래스는 setter가 필요하고, 변환대상 클래스는  
getter가 필요하다.  
1. source와 target의 필드 이름이 다를 경우 직접 지정할 수 있다.  
2. ignore를 통해 특정 필드는 변환되지 않도록 설정할 수 있다. target만 신경쓰면 된다. 
3. 기본적으로 deep mapping하는 코드까지 generation 해주긴하나, 기본적인 방식으로만 generation된다  
   (이름에 맞춰서 get/set). 그러므로 custom한 mapper가 필요하다면 위와 같이 선언해줘야 한다.  

install한 library로 빌드하면 @Mapper 인터페이스들을 찾아서 xxxImpl의 형태로 구현체를 모두 만든다.   
> 현재 componentModel을 spring으로 설정했기 때문에 생성되는 Imple은 스프링의 싱글톤 빈으로 관리된다.  
> (@Component 붙음)

생성된 구현체는 아래와 같다. 간단하다. 
```
@Override
public CatDTO toDto(Car entity) {
  if (entity == null) {
    return null;
  }
  
  CarDTO carDTO = new CarDTO();
  
  carDTO.setName(entity.getName());
  carDTO.setSeatCount(entity.numberOfSeats());
  carDTO.setOwner(ownerMapper.toDto(entity.getOwner()));
  
  return carDTO;
}
```
아래는 간단한 사용법이다.
```
//some service
public void save(CarDTO carDTO) {
  Car car = CarMapper.INSTANCE.toEntity(carDTO);
  
  em.persist(car);
}

public CarDTO select() { 
  Car car = em.find(Car.class, 1);
  
  return CarMapper.INSTANCE.toDTO(car);
}
```

---

# 정리
- 자바의 @Mapper가 있고 마이바티스에 @Mapper가 있다. 여기서는 자바의 @Ma  
pper를 말한다.   
Model Mapping을 아주 쉽게 해주는 Mapstruct라는 라이브러리를 알아보자.   
그 전에 Model mapping에 많이 사용되는 ModelMapper와의 간단한 차이를   
이야기해보면, Model Mapper는 Runtime 시점에 reflection으로 모델 매핑을  
하게 되는데 이렇게 런타임 시에 리플렉션이 자주 이루어진다면 앱 성능에 아주  
좋지 않다. 하지만 Mapstruct는 컴파일 타임에 매핑 클래스를 생성해줘 그 구현  
체를 런타임에 사용하는 것이기 때문에 앱 사이즈는 조금 커질 수 있지만 성능  
상 크게 이슈가 없어서 ModelMapper보다 더 부담없이 사용하기 좋다. 그리고   
아주 다양한 기능을 제공하기 때문에 조금 더 세밀한 매핑이 가능해진다.   
  
바로 간단하게 Mapstruct를 맞보자.  
1. build.gradle 추가  
2. java   
```java
@Data
@AllArgsConstructor(staticName="of")
@NoArgsConstructor
public class CarDto {
  private String name;
  private String color;
}

@Data
@AllArgsConstructor(staticName = "of")  
@NoArgsConstructor  
public class Car {
  private String modelName;
  private String modelColor;
}

@Mapper
public interface CarMapper {
  CarMapper INSTANCE = Mappers.getMapper(CarMapper.class);

  //source가 파라미터로 온 CarDto
  //target이 반환될 Car
  @Mapping(source = "name", target = "modelName")
  @Mapping(source = "color", target = "modelColor")
  Car to(CarDto carDto);
}
```
사실 간단한 것이라, 어노테이션만 보아도 어떠한 기능을 제공하는지 확실하다.  
하지만 각 클래스에 getter/setter 및 기본 생성자는 꼭 생성자주자(필드명이  
동일할 일은 많이 없겠지만, 각 객체의 필드명이 동일하면 @Mapping 어  
노테이션은 생략가능하다).  

# 하나의 객체로 합치기  
여러 객체의 필드값을 하나의 객체로 합치기가 가능하다. 특별히 다른 옵션을   
넣는 것은 아니다.   
```java
@Data
@AllArgsConstructor(staticName = "of")
@NoArgsConstructor  
public class UserDto {
  private String name;
}

@Data
@AllArgsConstructor(staticName = "of")
@NoArgsConstructor   
public class AddressDto {
  private String si;
  private Strin dong;
}

@Mapper
public interface UserInfoMapper {
  UserInfoMapper INSTANCE = Mappers.getMapper(UserInfoMapper.class);

  @Mapping(source = "user.name", target = "userName")
  @Mapping(source = "address.si", target = "si")
  @Mapping(source = "address.dong", target = "dong")
  UserInfo to(UserDto user, AddressDto address);
}
```

---

MapStruct는 클래스간 변환을 간편하게 해주는 라이브러리이다.(Car <> Car  
DTO).  
아래는 Mapper 선언법이다.   
```java
@Mapper(componentModel = "spring", uses = {
  OwnerMapper.class //3
})
public interface CarMapper {
  CarMapper INSTANCE = Mappers.getMapper(CarMapper.class);

  @Mapping(source = "numberOfSeats", target = "seatCount") //1
  CarDto toDto(Car car);

  @Mapping(target = "id", ignore = true) //2
  Car toEntity(CarDto dto);
}
```
보다시피 entity를 DTO로 변환해주는 작업을 한다  
(지금은 엔티티와 DTO를 예시로 들었지만 어떤 클래스든 상관없다).     
아래의 generate된 코드를 보면 알 수 있겠지만, 변환될 클래스는 setter가  
필요하고, 변환대상 클래스는 getter가 필요하다.   
1. source와 target의 필드 이름이 다를 경우 직접 지정할 수 있다.  
2. ignore를 통해 특정 필드는 변환되지 않도록 설정할 수 있다. target만   
신경쓰면 된다.   
3. 기본적으로 deep mapping하는 코드까지 generation 해주긴 하나, 기본적인  
방식으로만 generation된다(이름에 맞춰서 get/set). 그러므로 custom한   
mapper가 필요하다면 위와 같이 선언해줘야 한다.  
install한 library로 빌드하면 @Mapper 인터페이스들을 찾아서 xxxImpl의   
형태로 구현체를 모두 만든다.   

> 현재 componentModel을 spring으로 설정했기 때문에 생성되는 impl은   
> 스프링의 싱글톤 빈으로 관리된다(@Component 붙음).  






























  
