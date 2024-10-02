# Set Up an App Dev Environment on Google Cloud: Challenge Lab

## Task 1: Create a Bucket
1. No **Menu de Navegação**, clique em **Cloud Storage**>**Buckets**.
2. Clique em **CREATE**.
3. Na tela **Create a Bucket**, preencha o nome sugerido pelo seu lab.
4. Clique em **CREATE** e depois em **CONFIRM**.

## Task 2: Create a Pub/Sub Topic
1. No **Menu de Navegação**, clique em **VIEW ALL PRODUCTS** e busque pela opção **Pub/Sub** na barra de pesquisa.
2. Na opção **Topics** em Pub/Sub, clique em **CREATE TOPIC**.
3. No campo **Topic ID**, preencha com o nome definido pelo lab.
4. Clique em **CREATE**.

## Task 3: Create the Thumbnail Cloud Run Function
1. No **Menu de Navegação**, clique em **VIEW ALL PRODUCTS** e busque pela opção **Cloud Run Function** na barra de pesquisa.
2. Clique em **CREATE FUNCTION**.
3. No campo **Environment**, selecione a opção **Cloud Run Function**.
4. Em **Function Name**, preencha com o nome definido pelo lab.
5. Em **Region**, preencha com a região definida pelo lab.
6. Em **Trigger Type**, selecione **Cloud Storage**.
7. Em **Bucket**, preencha com o nome do bucket criado anteriormente, clique em **BROWSE** e **SELECT**.
8. Na área **Runtime, build, connections and security settings**, em **Maximum Number of Instances**, coloque 5.
9. Clique no botão **NEXT** e depois em **ENABLE**.
10. Em **Entry Endpoint**, preencha com o nome da Cloud Function sugerido pelo seu lab.
11. Substitua o código de `index.js` pelo primeiro código apresentado na tarefa 3.
12. Substitua o código de `package.json` pelo segundo código apresentado na tarefa 3.
13. Clique em **DEPLOY**.

## Task 4: Test the Infrastructure
1. Volte para **Cloud Storage**>**Buckets**.
2. Clique no nome do bucket criado neste lab.
3. Como alternativa, é possível utilizar a imagem sugerida na tarefa 4 para fazer o upload dela no bucket. Salve-a em seu computador.
4. Clique em **UPLOAD > UPLOAD FILE** e selecione a imagem.
5. Clique em **REFRESH**.

## Task 5: Remove the Previous Cloud Engineer
1. No menu de Navegação, vá para **IAM & Admin**>**IAM**.
2. Clique em **View By Roles**.
3. Em **Viewer**, clique no ícone de lápis do outro usuário indicado pelo lab.
4. Clique no ícone de lixeira e em **SAVE**.

**Congratulations!**
