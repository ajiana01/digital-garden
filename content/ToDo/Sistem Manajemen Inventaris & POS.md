### Board Game Inventory ERD

| Distributor |      <       |  <  |
| :---------: | :----------: | :-: |
|   string    |      id      | PK  |
|   string    |     name     |     |
|   string    | contact_info |     |

| Boardgame |       <        |  <  |
| :-------: | :------------: | :-: |
|  string   |       id       | PK  |
|  string   |      ssku      |     |
|  string   |      name      |     |
|  string   | distributor_id | FK  |

| StockTransaction |     <     |  <  |        <        |
| :--------------: | :-------: | :-: | :-------------: |
|      string      |    id     | PK  |                 |
|      string      |  game_id  | FK  |                 |
|      string      |   type    |     | TransactionType |
|       int        | quantity  |     |                 |
|      float       | unit_cost |     |    UnitCost     |

{
  "name": "Dadu Nusantara Distributor",
  "email": "halo@dadunusantara.com",
  "phone": "081234567890"
}


{
  "title": "Catan Base Game",
  "sku": "BG-CTN-01",
  "sellingPrice": 650000,
  "distributorId": 1
}