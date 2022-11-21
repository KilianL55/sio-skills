## Compétences utilisés :

- [Utilisation de composants d’accès aux données](https://github.com/KilianL55/sio-skills/issues/9)
- [Exploitation des ressources du cadre applicatif](https://github.com/KilianL55/sio-skills/issues/9)

## Utilisation de composants d'accès aux données : 

### Création :

Pour créer une table avec le Framework Spring Boot on utilise le module JPA Database. Il faut donc implémente une class métier (Entity)

Exemple d'implémentation d'une class Entity :

```Java
@Entity
public class User {
    @Id
    private int id;
    private String name;
    private int age
}
```
Cette implémentation va donc créer une table sous cette forme :

ID | Name | Age
--- | --- | ---

Pour exploiter les données d'une base de données, il faut utlisé des repositories.

Exemple d'implémentation d'un repository : 

```Java
public interface IRepoUser extends CrudRepository<User, Integrer> {
    
    public User findByName();
    
    @Query("SELECT u FROM User u WHERE u.age = :name")
    public User retrieveByAge(@Param("age") int age);
}
```
Dans notre exemple `extends CrudRepository<User, Integrer>` permet d'intégrer les méthodes fourni par l'interface CrudRepository :

- save()
- saveAll()
- findById()
- existsById()
- indAll()
- findAllById()
- count()
- deleteById()
- delete()
- deleteAllById()
- deleteAll()
- deleteAll()

Sinon vous pouvez impléter vos propres méthodes : 

- `public User findByName();` qui va donc chercher le User par son nom.
- Il est possible d'utiliser l'annotation @Query pour créer une requête en JPQL ou SQL Natif
```Java
@Query("SELECT u FROM User u WHERE u.age = :name")
public User retrieveByAge(@Param("age") int age);
```

### Utilisation du Repository :

Injection du Repository : 

```Java
@Controller
public class UserController {
 
    @Autowired
    private IRepoUser userRepo;
}
```

Création d'un user en base de données :

```Java
User user = new User()
user.setName("UserName");
userRepo.save(user);
```

Lecture d'un user par son id :

```Java
User user;
Optional<User> optUser = repoUser.findById(id);
if (optUser.isPresent()) {
    user=optUser.get();
}
```

Suppression d'un user par son id :

```Java
int id = user.getId();
userRepo.deleteById(id);
```

Mise à jour de données d'un user existant :

```Java
User user;
Optional<User> optUser = repoUser.findById(id);
if (optUser.isPresent()) {
    user=optUser.get();
    user.setName("newUserName");
    repo.save(user);
}
```



