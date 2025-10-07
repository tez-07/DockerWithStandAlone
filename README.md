# 🐳 Docker with Selenium Grid - Quick Guide

This guide summarizes how to integrate **Docker** with **Selenium Grid** for running automated tests in parallel. It covers **Windows** (Selenium) and **Mac ARM64** (Seleniarm) setups.

---

## **Steps to Setup and Run Tests**

- **Pull Docker Images**
  - Windows (Intel/AMD):  
    `docker pull selenium/standalone-chrome:latest`
  - Mac (ARM64, M1/M2):  
    `docker pull seleniarm/standalone-chromium:latest`

- **Run Containers**
  - Windows:  
    `docker run -d -p 4444:4444 selenium/standalone-chrome:latest`
  - Mac ARM64:  
    `docker run -d -p 4444:4444 -p 7900:7900 --shm-size="2g" seleniarm/standalone-chromium:latest`
  - Notes:  
    - First port (`4444`) → host port  
    - Second port (`4444` or `7900`) → container port  

- **Check Running Containers**  
  `docker ps`

- **Stop Containers (Mac Example)**  
  `docker stop <container_id>`

- **Important Points**
  - Only running containers will execute test scripts.  
  - Stopped containers → scripts fail.  
  - Selenium 4 deprecated `DesiredCapabilities`, use `ChromeOptions` / `FirefoxOptions`.

---

## **Maven Project Setup for Remote WebDriver**

- Concept:
  - Use **ChromeOptions** or **FirefoxOptions** for browser settings (normal, headless, etc.).  
  - Point **RemoteWebDriver** URL to the Selenium container (Docker).  
  - Connect and control the browser remotely.

```java
// Browser options
ChromeOptions options = new ChromeOptions(); // or FirefoxOptions options = new FirefoxOptions();

// Selenium Grid container URL
URL url = new URL("http://localhost:4444/wd/hub");

// Connect and control browser remotely
RemoteWebDriver driver = new RemoteWebDriver(url, options);
