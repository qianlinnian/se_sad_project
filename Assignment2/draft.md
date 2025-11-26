┌─────────────────────────────────────────────────────────────────────────────┐
│               CAMPUS FOOD ORDERING & RESERVATION SYSTEM                     │
└─────────────────────────────────────────────────────────────────────────────┘


┌──────────────────────────┐
│    <<entity>>            │
│      Student             │
├──────────────────────────┤
│ - studentId:String       │
│ - name:String            │
│ - password:String        │
│ - email:String           │ ◄── ✅ 新增
│ - balance:BigDecimal     │
├──────────────────────────┤
│ + validateLogin():bool   │
│ + checkBalance():BigDec  │
│ + deduct(amount):bool    │
│ + recharge(amount):void  │ ◄── ✅ 新增
└──────────────┬───────────┘
               │ 1..*
               │
┌──────────────▼──────────────────────────────────────────┐
│                  <<entity>>                             │
│                    Order                                │
├──────────────────────────────────────────────────────────┤
│ - orderId:String                                         │
│ - studentId:String                                       │
│ - restaurantId:String                                    │
│ - dishes:List<Dish>                                      │
│ - quantities:List<Integer>                               │
│ - totalAmount:BigDecimal                                 │
│ - status:String (pending/confirmed/completed/cancelled) │
│ - orderTime:DateTime                                     │
├──────────────────────────────────────────────────────────┤
│ + calculateTotal():BigDecimal                            │
│ + addDish(dish:Dish, qty:int):void                       │
│ + removeDish(dishId:String):void                         │
│ + updateStatus(status:String):void                       │
│ + getOrderSummary():String                               │
│ + cancel():boolean                                       │
└──────────────┬──────────────────────────┬────────────────┘
               │ *.*                      │ 1
               │                          │
               │                ┌─────────▼──────────────────┐
               │                │   <<entity>>               │
               │                │    Payment                 │ ◄── ✅ 独立类
               │                ├────────────────────────────┤
               │                │ - paymentId:String         │
               │                │ - orderId:String           │
               │                │ - method:String            │
               │                │ - amount:BigDecimal        │
               │                │ - status:String            │
               │                │ - paymentTime:DateTime     │
               │                ├────────────────────────────┤
               │                │ + processPayment():bool    │
               │                │ + refund():boolean         │
               │                │ + getStatus():String       │
               │                └────────────────────────────┘
               │
               │                ┌─────────────────────────────┐
               │                │   <<entity>>                │
               │                │   Reservation              │ ◄── ✅ 独立类
               │                ├─────────────────────────────┤
               │                │ - reservationId:String      │
               │                │ - orderId:String            │
               │                │ - reservationTime:DateTime  │
               │                │ - status:String             │
               │                │ - numberOfPeople:int        │
               │                ├─────────────────────────────┤
               │                │ + confirm():boolean         │
               │                │ + cancel():boolean          │
               │                │ + update(time):boolean      │
               │                │ + getStatus():String        │
               │                └─────────────────────────────┘
               │
               ▼
┌──────────────────────────────────────────┐
│         <<entity>>                       │
│            Dish                          │
├──────────────────────────────────────────┤
│ - dishId:String                          │
│ - restaurantId:String                    │
│ - name:String                            │
│ - description:String                     │
│ - price:BigDecimal                       │
│ - category:String                        │
│ - isAvailable:boolean                    │
│ - imageUrl:String                        │
├──────────────────────────────────────────┤
│ + isAvailable():boolean                  │
│ + setAvailable(bool):void                │
│ + getDishInfo():String                   │
└──────────────┬───────────────────────────┘
               │ 1.*
               │
               ▼
┌──────────────────────────────────────────┐
│         <<entity>>                       │
│         Restaurant                       │
├──────────────────────────────────────────┤
│ - restaurantId:String                    │
│ - name:String                            │
│ - location:String                        │
│ - phone:String                           │
│ - email:String                           │
│ - operatingHours:String                  │
│ - rating:Double                          │
├──────────────────────────────────────────┤
│ + getDishes():List<Dish>                 │
│ + isOpenNow():boolean                    │
│ + getRestaurantInfo():String             │
│ + updateRating(rating):void              │
└──────────────────────────────────────────┘


┌──────────────────────────────────────────────────────────────────┐
│                    <<boundary>>                                  │
│                 SystemInterface                                  │
├──────────────────────────────────────────────────────────────────┤
│ - currentUser:Student                                            │
│ - isLoggedIn:boolean                                             │
├──────────────────────────────────────────────────────────────────┤
│ + showLoginForm():void                                           │
│ + login(studentId:String, password:String):Student              │
│ + showMessage(msg:String):void                                   │
│ + logout():void                                                  │
└──────────────┬───────────────────────────────────────────────────┘
               │ uses
               │
               ▼
┌──────────────────────────────────────────────────────────────────┐
│                    <<control>>                                   │
│                 OrderController                                  │
├──────────────────────────────────────────────────────────────────┤
│ - orderRepository:OrderRepository                                │
│ - paymentService:PaymentService                                  │
│ - reservationService:ReservationService                          │
├──────────────────────────────────────────────────────────────────┤
│ + createOrder(studentId:String):Order                            │
│ + addDishToOrder(order:Order, dish:Dish, qty:int):void           │
│ + submitOrder(order:Order):boolean                               │
│ + cancelOrder(orderId:String):boolean                            │
│ + getOrderHistory(studentId:String):List<Order>                  │
└──────────────┬───────────────────────────────────────────────────┘
               │ uses
               │
               ▼
┌──────────────────────────────────────────────────────────────────┐
│                    <<boundary>>                                  │
│                 OrderingInterface                                │
├──────────────────────────────────────────────────────────────────┤
│ - currentStudent:Student                                         │
│ - currentOrder:Order                                             │
├──────────────────────────────────────────────────────────────────┤
│ + showRestaurants():void                                         │
│ + showMenu(restaurantId:String):void                             │
│ + addToCart(dish:Dish, qty:int):void                             │
│ + showCart():void                                                │
│ + checkout():void                                                │
│ + showPaymentOptions():void                                      │
│ + showReservationForm():void                                     │
│ + showOrderHistory():void                                        │
└──────────────┬───────────────────────────────────────────────────┘
               │ uses
               │
               ▼
┌──────────────────────────────────────────────────────────────────┐
│                    <<control>>                                   │
│                 MenuController                                   │
├──────────────────────────────────────────────────────────────────┤
│ - restaurantRepository:RestaurantRepository                      │
│ - dishRepository:DishRepository                                  │
│ - cacheManager:CacheManager                                      │
├──────────────────────────────────────────────────────────────────┤
│ + getAllRestaurants():List<Restaurant>                           │
│ + getDishesByRestaurant(restaurantId:String):List<Dish>          │
│ + getDishById(dishId:String):Dish                                │
│ + searchDishes(keyword:String):List<Dish>                        │
│ + filterByCategory(category:String):List<Dish>                   │
└──────────────────────────────────────────────────────────────────┘
