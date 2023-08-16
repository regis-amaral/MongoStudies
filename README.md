1. **Criação de Banco de Dados e Coleção:**
    - Crie um banco de dados chamado "ravin";
    - Crie uma coleção chamada "menus" dentro do banco de dados "ravin";
        - O menu deve conter os seguintes campos:
            - Code (Required)
            - Name (Required)
            - Description
            - Produtos (Required)
        - Cada produto deve conter os seguintes campos:
            - Code (Required)
            - Name (Required)
            - Price (Required)
            - PreparationTime
            - ProductType (DRINK, FOOD, DESSERT) (Required)
            - Comments
            - HasActive (Required)
2. **Inserção de Documentos:**
    - Crie um novo documento que contenha os menus e os produtos de cada menu;
    - Insira 10 produtos em cada menu;
3. **Consulta de Documentos:**
    - Recupere todos os documentos da coleção "menus".

    ```
    db.menus.find()
    ```

    - Recupere todos os produtos que são DRINKS;

    ```
    db.menus.aggregate([
        {
            $unwind: "$products"
        },
        {
            $match: {"products.productType": "DRINK"}
        },
        {
            $project: {_id: 0, product: "$products"}
        }
    ])
    ```
    - Recupere todos os projetos que custem mais que 50.00
4. **Atualização de Documentos:**

    ```
    db.menus.updateOne({"code": "MENU1"}, {$set: {name: "Cardápio de BlumenHell"}})
    ```

    - Atualize os o valor de todos DESSERTS acrescentando 10% do valor anterior;
    ```
    db.menus.updateMany(
    { "products.productType": "DESSERT" },
    { $mul: { "products.$[x].price": 1.1 }},
    { arrayFilters: [{"x.productType":  "DESSERT"}]}
    );
    ```

5. **Remoção de Documentos:**
    - Remova 1 produto de cada cardápio;
    ```
    db.menus.updateMany({}, {$pop: {"products": -1}})
    ```
    - Remova todos os produtos que custem menos de 1.00

6. **Ordenação e Limite:**
    - Recupere os documentos da coleção "menu" ordenados por idade de forma descendente.
    db.menus.find({}).sort({"code": -1})
    - Recupere os três documentos mais novos da coleção "users".

7. **Índices Simples:**
    - Crie um índice ascendente na chave "nome" na coleção "produto".
    ```
    db.menus.createIndex({"products.code": 1})
    ```