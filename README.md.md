# Blog Platform

Kompleksowa platforma blogowa zbudowana przy użyciu Spring Boot, Hibernate i MySQL. System umożliwia tworzenie, edytowanie i publikowanie postów blogowych, zarządzanie komentarzami, tagowanie treści oraz śledzenie statystyk odwiedzin.

## Funkcjonalności

### Zarządzanie użytkownikami
- Rejestracja i logowanie użytkowników
- Zarządzanie profilem użytkownika
- Różne poziomy dostępu (administrator, autor, czytelnik)

### Zarządzanie postami
- Tworzenie, edytowanie i usuwanie postów
- Publikowanie i wycofywanie publikacji
- Wersjonowanie treści
- Wsparcie dla formatowania tekstu i multimediów

### Komentarze
- Dodawanie komentarzy do postów
- Moderacja komentarzy
- Hierarchiczna struktura komentarzy (odpowiedzi)

### System tagów
- Dodawanie tagów do postów
- Filtrowanie postów według tagów
- Chmura tagów i sugestie powiązanych treści

### Statystyki
- Śledzenie odsłon postów
- Liczenie unikalnych odwiedzających
- Raporty aktywności według dat
- Analiza popularności treści

## Technologie

- **Backend**: Spring Boot 3.1, Spring Security, Spring Data JPA
- **Baza danych**: MySQL 8.0
- **Uwierzytelnianie**: JWT (JSON Web Token)
- **Dokumentacja API**: Swagger/OpenAPI 3.0
- **Testy**: JUnit 5, Mockito
- **Konteneryzacja**: Docker, Docker Compose

## Wymagania systemowe

- Java 17+
- Maven 3.8+
- MySQL 8.0+
- Docker (opcjonalnie)

## Konfiguracja i uruchomienie

### Konfiguracja bazy danych

1. Utwórz bazę danych MySQL:

```sql
CREATE DATABASE blog_platform;
CREATE USER 'blog_user'@'localhost' IDENTIFIED BY 'yourpassword';
GRANT ALL PRIVILEGES ON blog_platform.* TO 'blog_user'@'localhost';
FLUSH PRIVILEGES;
```

2. Skonfiguruj połączenie z bazą danych w `application.properties`:

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/blog_platform
spring.datasource.username=blog_user
spring.datasource.password=yourpassword
spring.jpa.hibernate.ddl-auto=update
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL8Dialect
```

### Uruchamianie aplikacji

#### Za pomocą Maven

```bash
mvn clean install
mvn spring-boot:run
```

#### Za pomocą Docker

```bash
docker-compose up -d
```

Aplikacja będzie dostępna pod adresem: http://localhost:8080

### Dostęp do API

Po uruchomieniu aplikacji dokumentacja API Swagger będzie dostępna pod adresem:
http://localhost:8080/swagger-ui.html

## Struktura API

### Autentykacja

- `POST /api/auth/register` - Rejestracja nowego użytkownika
- `POST /api/auth/login` - Logowanie użytkownika i generowanie tokenu JWT

### Użytkownicy

- `GET /api/users` - Pobierz wszystkich użytkowników
- `GET /api/users/{id}` - Pobierz użytkownika według ID
- `GET /api/users/username/{username}` - Pobierz użytkownika według nazwy
- `PUT /api/users/{id}` - Aktualizuj użytkownika
- `DELETE /api/users/{id}` - Usuń użytkownika

### Posty

- `GET /api/posts` - Pobierz wszystkie posty
- `GET /api/posts/published` - Pobierz wszystkie opublikowane posty
- `GET /api/posts/{id}` - Pobierz post według ID
- `GET /api/posts/author/{userId}` - Pobierz posty według autora
- `GET /api/posts/tag/{tagId}` - Pobierz posty według tagu
- `POST /api/posts` - Utwórz nowy post
- `PUT /api/posts/{id}` - Aktualizuj post
- `DELETE /api/posts/{id}` - Usuń post
- `PUT /api/posts/{id}/publish` - Opublikuj post
- `PUT /api/posts/{id}/unpublish` - Wycofaj publikację posta
- `PUT /api/posts/{postId}/tags/{tagId}` - Dodaj tag do posta
- `DELETE /api/posts/{postId}/tags/{tagId}` - Usuń tag z posta

### Komentarze

- `GET /api/comments/post/{postId}` - Pobierz komentarze do posta
- `GET /api/comments/user/{userId}` - Pobierz komentarze użytkownika
- `GET /api/comments/{id}` - Pobierz komentarz według ID
- `POST /api/comments` - Dodaj nowy komentarz
- `PUT /api/comments/{id}` - Aktualizuj komentarz
- `DELETE /api/comments/{id}` - Usuń komentarz

### Tagi

- `GET /api/tags` - Pobierz wszystkie tagi
- `GET /api/tags/{id}` - Pobierz tag według ID
- `GET /api/tags/name/{name}` - Pobierz tag według nazwy
- `POST /api/tags` - Utwórz nowy tag
- `PUT /api/tags/{id}` - Aktualizuj tag
- `DELETE /api/tags/{id}` - Usuń tag

### Statystyki

- `GET /api/statistics/post/{postId}` - Pobierz statystyki dla posta
- `GET /api/statistics/{id}` - Pobierz statystyki według ID
- `GET /api/statistics/post/{postId}/views` - Pobierz całkowitą liczbę wyświetleń posta
- `GET /api/statistics/post/{postId}/unique-visitors` - Pobierz całkowitą liczbę unikalnych odwiedzających
- `POST /api/statistics/post/{postId}/view` - Zarejestruj wyświetlenie posta

## Bezpieczeństwo

System używa JWT (JSON Web Token) do autentykacji. Po zalogowaniu, token JWT powinien być dołączany do każdego żądania w nagłówku `Authorization`:

```
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

## Testy

### Uruchamianie testów jednostkowych

```bash
mvn test
```

### Uruchamianie testów integracyjnych

```bash
mvn verify
```

## Środowiska

Aplikacja obsługuje różne środowiska konfiguracyjne:

- `application-dev.properties` - Konfiguracja dla środowiska deweloperskiego
- `application-prod.properties` - Konfiguracja dla środowiska produkcyjnego

Aby uruchomić aplikację w konkretnym środowisku:

```bash
mvn spring-boot:run -Dspring-boot.run.profiles=dev
```

lub

```bash
java -jar blog-platform.jar --spring.profiles.active=prod
```

## Struktura projektu

Projekt jest zorganizowany zgodnie z najlepszymi praktykami Spring Boot:

- `config/` - Klasy konfiguracyjne
- `controller/` - Kontrolery REST API
- `dto/` - Obiekty transferu danych
- `entity/` - Encje bazodanowe
- `exception/` - Obsługa wyjątków
- `repository/` - Repozytoria danych
- `security/` - Komponenty bezpieczeństwa
- `service/` - Warstwa usług

## Rozwój projektu

### Dodawanie nowych funkcji

1. Utwórz nową gałąź z `main`
2. Zaimplementuj funkcję
3. Napisz testy
4. Wyślij pull request

### Konwencje kodowania

- Używaj standardowego formatowania kodu Java
- Dokumentuj metody publiczne za pomocą JavaDoc
- Stosuj zasady SOLID
- Testuj wszystkie nowe funkcje

## Rozwiązywanie problemów

### Problemy z bazą danych

- Upewnij się, że serwer MySQL jest uruchomiony
- Sprawdź poprawność danych logowania w `application.properties`
- Sprawdź uprawnienia użytkownika bazy danych

### Problemy z uruchomieniem aplikacji

- Sprawdź logi aplikacji
- Sprawdź, czy port 8080 nie jest zajęty przez inną aplikację
- Sprawdź kompatybilność wersji Java

## Licencja

Ten projekt jest dostępny na licencji MIT. Szczegóły można znaleźć w pliku LICENSE.

## Kontakt

W przypadku pytań lub sugestii, prosimy o kontakt z zespołem deweloperskim.
