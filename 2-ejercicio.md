# Ejercicio
Configurar SonarQube utilizando Docker Compose, para esto necesitas dos servicios:
- Servicio: SonarQube
- Desde el host es necesario acceder a SonarQube por lo que necesitas mapear el puerto correspondiente.
- Servicio: PostgreSQL (existen otras opciones: Microsoft SQL Server, Oracle)
- Coloca un healtcheck para cada uno de los servicios.
- Los dos servicios deben pertenecer a uan red de tipo bridge
- Investiga cuáles son los volúmenes necesarios para cada servicio
- Investiga cuáles son las variables de entorno para que los servicios funcionen de manera adecuada.

### compose.yaml

```
version: '3.9'

services:
  sonarqube:
    image: sonarqube:latest
    container_name: sonarqube
    ports:
      - 9000:9000
    environment:
      SONARQUBE_JDBC_URL: jdbc:postgresql://postgres:5432/sonar
      SONARQUBE_JDBC_USERNAME: sonar
      SONARQUBE_JDBC_PASSWORD: sonar
    volumes:
      - sonarqube-conf:/opt/sonarqube/conf
      - sonarqube-data:/opt/sonarqube/data
      - sonarqube-logs:/opt/sonarqube/logs
      - sonarqube-extensions:/opt/sonarqube/extensions
    networks:
      - sonarnet
    healthcheck:
      test: ["CMD-SHELL", "curl -f http://localhost:9000 || exit 1"]
      interval: 1m
      timeout: 10s
      retries: 3
      start_period: 1m

  postgres:
    image: postgres:latest
    container_name: postgres
    environment:
      POSTGRES_USER: sonar
      POSTGRES_PASSWORD: sonar
      POSTGRES_DB: sonar
    volumes:
      - postgresql-data:/var/lib/postgresql/data
    networks:
      - sonarnet
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U sonar"]
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 30s

volumes:
  sonarqube-conf: {}
  sonarqube-data: {}
  sonarqube-logs: {}
  sonarqube-extensions: {}
  postgresql-data: {}

networks:
  sonarnet:
    driver: bridge
```

## http://localhost:9000.

![image](https://github.com/MaxCar31/2024A-ISWD633-Practica5/assets/141116497/1fc6f238-d2fc-485f-a612-a40511997624)

![image](https://github.com/MaxCar31/2024A-ISWD633-Practica5/assets/141116497/b14463db-ccbf-46ae-85cd-7d899b69f189)

![image](https://github.com/MaxCar31/2024A-ISWD633-Practica5/assets/141116497/9d58a800-0cbf-4aa5-b21a-95c159b7e629)

![image](https://github.com/MaxCar31/2024A-ISWD633-Practica5/assets/141116497/ae295c36-8520-4710-b7ff-a7bdd4a67d41)

![image](https://github.com/MaxCar31/2024A-ISWD633-Practica5/assets/141116497/f8fc34ef-9b8a-46d6-b4f9-93c613f0f07d)

