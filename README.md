Задание.
Реализуйте удаление пользователей.
Подумать, где должен находиться метод createUser из UserView и если получится, вынести его в нужный слой.
Вынести логику dao в нужный слой (репозиторий), а от слоя dao избавится физически. ВАЖНО: ознакомиться с статьей: https://habr.com/ru/articles/263033/
На выбор (не обязательно):
1)подумайте как оптимизировать код приложения (например, хэшировать все данные, а в файл писать только при выходе из приложения)
2) Дописать код для оставшихся команд в Commands (в NONE можно реализовать сохранение списка USer)
ИЛИ ВНЕСИТЕ СВОИ ИЗМЕНЕНИЯ В ПРОЕКТ, КОТОРЫЕ КАЖУТЬСЯ ЛОГИЧНЫМИ ВАМ.
Пожалуйста о том что изменили, напишите в комментариях(например в ридми)
ОБЯЗАТЕЛЬНО прочтите статьи:
- https://javarush.com/groups/posts/2536-chastjh-7-znakomstvo-s-patternom-mvc-model-view-controller

https://habr.com/ru/articles/263033/ ****Не обязательное!!! Создать калькулятор для работы с числами, организовать меню, добавив в него систему логирования
Формат сдачи: ссылка на гитхаб

РЕАЛИЗАЦИЯ
1. УДАЛЕНИЕ ПОЛЬЗОВАТЕЛЕЙ
2. В UserRepository(repository) переопределяем метод public boolean delete(Long id)
3. В UserController(controller) прописываем метод public boolean deleteUser(Long userid)
4. В UserView(view) добавляем стр 44-47 команда DELETE
5. 
2. ПОДУМАТЬ, ГДЕ ДОЛЖЕН НАХОДИТЬСЯ МЕТОД createUser ИЗ UserView И ВЫНЕСТИ ЕГО В НУЖНЫЙ СЛОЙ
     Перенесла в UserController(controller). Сделала метод из private в public и добавила метод private String scan(String message) {
     В UserView необходимо внести изменения в строку 28 и 50: userController.createUser()
   
3.Вынести логику dao в нужный слой (репозиторий), а от слоя dao избавится физически. 
Слой dao логичнее всего перенести в repository
1.Из интерфейса Operation необходимо все команды перенести в интерфейс GBRepository
2.Далее перенести код из FileOperation в UserRepository
Кусок кода в UserRepository, который был до изменений:
public class UserRepository implements GBRepository {
    private final UserMapper mapper;
    private final FileOperation operation;

    public UserRepository(FileOperation operation) {
        this.mapper = new UserMapper();
        this.operation = operation;
    }
    После изменений код бужет выглядеть так:
    public class UserRepository implements GBRepository {
    private final UserMapper mapper;
    private final String fileName;


    public UserRepository(String fileName) {
        this.fileName = fileName;
        this.mapper = new UserMapper();
        try (FileWriter writer = new FileWriter(fileName, true)) {
            writer.flush();
        } catch (IOException e) {
            System.out.println(e.getMessage());
        }
    }
Код из FileOperation в части методов readAll и saveAll переносится в UserRepository без изменений копированием
3. В Main изменятся строки:
FileOperation fileOperation = new FileOperation(DB_PATH);
GBRepository repository = new UserRepository(fileOperation);
В измененном виде останется 
GBRepository repository = new UserRepository(DB_PATH);
4. во всем проекте убираем import notebook.model.dao.impl.FileOperation;
5. удаляем слой dao
6. тестируем Main на работоспособность - у меня все получилось!

Идем пить кофе:)
     
