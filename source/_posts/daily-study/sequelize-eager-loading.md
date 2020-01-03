---
title: sequelize include 팁 + unknown column 오류 해결
date: 2020-01-04 00:25:12
author: Roseline Song
categories: Daily-Study
tags: 기록 공부
---

### 📔 레퍼런스

- [sequelize 공식문서: eager loading](https://sequelize.org/master/manual/models-usage.html#eager-loading)
- [stackOverFlow: nested include](https://stackoverflow.com/questions/33941943/nested-include-in-sequelize)

### required: true

required: true 옵션이 없으면 기본적으로 left join을 사용하게 된다.
관계에 따라서는, 중복된 row들이 조회될 수 있으므로 inner join으로 가져오려면 `required: true` 옵션을 사용한다.

```javascript
Company.findAll({
  include: [
    { model: Company.Product, required: true }
  ]
  where: {
    ...
  }
})
```

### 'Unknown columns in ...' 오류

```javascript
// Unknown columns 오류!
Company.findAll({
  include: [
    { model: Company.Product, as: "products" }
  ]
  where: {
    productName: { // Product 모델의 name 컬럼
      [Op.substring]: filterBy.productName
    }
  }
})
```

include를 써서 join query를 날리는 경우 위와 같이 where절을 쓰면 Unknown columns 오류가 난다. raw query를 살펴보면 아래처럼 `join이 실행되기도 전에 조인 테이블의 컬럼으로 where절에서 필터링`된다. 

```sql
SELECT (
    product.id,
    product.name, 
    product.company_id, 
    company.id,
    company.name,
    ...
) FROM (
    SELECT company.id, company.name, ... 
    FROM company
    WHERE product.name LIKE '%blabla%' -- join하기 전에 product 테이블의 name컬럼으로 필터링할 수 없으므로 Error
) INNER JOIN product on product.company_id = company.id;
```

정상적으로 필터링하려면 두가지 방법이 있다.

- include내 객체에 where절을 추가하거나
- `$nested.column$`를 이용해서 where절을 가장 바깥에 위치시킨다.

**1. include내 객체에 where절 추가**

```javascript
Company.findAll({
  include: [
    { 
      model: Company.Product, 
      where: {
        productName: {
          [Op.substring]: filterBy.productName
        }
      }
    }
  ]
})
```

**2. 최상위 레벨의 where 절 (Top level where clause)**

언뜻 보기에는 첫번째 방법이 나은 것 같지만, include할 모델이 여러 개라면 include내 객체마다 where절이 추가되면서 코드가 복잡해진다. 조인을 모두 끝낸 후, 가장 바깥에 있는 where절로 한번에 필터링한다.


```javascript
Company.findAll({
  include: [
    { 
      model: Company.Product, 
      as: 'products',
    }
  ],
  where: {
    // 모델명을 그대로 적지 않고, alias를 쓴다
    // 컬럼은 DB에 있는 컬럼(snakecase) 이름 사용한다
    [`$products.name$`]: {
      [Op.substring]: filterBy.productName
    }
  }
})
```

### 중첩 include (nested include)

예를 들어 `Company - Product`, `Product - ProductType`과 같이 association이 정의되어 있다. `ProductType = "LifeStyle"`인(생활용품을 판매하는) Company의 리스트를 조회한다면, Company에서 중첩 include문을 사용해 ProductType에 접근할 수 있다.

```javascript
Company.findAll({
  include: [
    { 
      model: Company.Product, 
      include: [
        { 
          model: Company.Product.ProductType, 
          where: {
              type: "LifeStyle",
          }
        }
      ] 
    }
  ]
})
```

### separate: true

`separate: true`: include내 여러 join이 들어가는 경우, 각 include별로 쿼리를 따로 날리고 싶은 경우에 사용한다. ([belongsToMany와 관련해 이슈가 나온지 꽤 지났지만, 아직 hasMany 관계에서만 사용 가능하다](https://github.com/sequelize/sequelize/issues/4376))

```javascript
Company.findAll({
  include: [
    { model: Company.Product, separate: true },
    { model: Company.Employee, separate: true},
    ...(manyAssociations),
  ]
})
```

### connection 테이블

association으로도 model과 똑같이 join할 수 있다. 그러나 연결을 위한 커넥션 테이블의 경우, association으로 직접 내보이기보다는 model을 써서 원래 조회하고자 하는 테이블을 써주는 게 좋을 것 같다.


```javascript
Company.findAll({
  include: [
    { association: Company.RegionConnection },
  ]
})

Company.findAll({
  include: [
    { 
      model: Company.Region, 
      as: "regions", 
      through: CompanyRegionConnection,
    },
  ]
})
```
