# GLFW quick reference for SCOP

Questa breve guida raccoglie le funzioni di GLFW più utili per soddisfare i requisiti del progetto SCOP: finestra e contesto OpenGL, gestione input/eventi e controllo del rendering.

## 1. inizializzazione e chiusura
- `int glfwInit(void)`
  - Deve essere chiamata per prima. Restituisce 0 se l'inizializzazione fallisce (es. librerie mancanti).
- `void glfwTerminate(void)`
  - Rilascia tutte le risorse allocate da GLFW. Richiamala prima di uscire dal programma.
- `void glfwSetErrorCallback(GLFWerrorfun cbfun)`
  - Registra una funzione che riceve codice e descrizione dell'errore, utile per il debug durante la difesa.

## 2. creazione della finestra / contesto
- `void glfwWindowHint(int hint, int value)`
  - Imposta proprietà del contesto prima di creare la finestra (es. `GLFW_CONTEXT_VERSION_MAJOR`, `GLFW_OPENGL_PROFILE`, `GLFW_SAMPLES` per MSAA, `GLFW_RESIZABLE`).
- `GLFWwindow *glfwCreateWindow(int width, int height, const char *title, GLFWmonitor *monitor, GLFWwindow *share)`
  - Crea la finestra principale. Puoi passare `NULL` per monitor/ share se non ti servono fullscreen o context sharing.
- `void glfwMakeContextCurrent(GLFWwindow *window)`
  - Associa il contesto della finestra al thread corrente; chiamala prima di iniziare a fare chiamate OpenGL.
- `void glfwSwapInterval(int interval)`
  - Imposta il V-Sync (1 = sincronizzato, 0 = disattivato). Utile per evitare tearing durante la rotazione dell’oggetto.

## 3. ciclo principale ed eventi
- `int glfwWindowShouldClose(GLFWwindow *window)`
  - Restituisce un flag che indica se la finestra deve chiudere; usalo come condizione del loop principale.
- `void glfwPollEvents(void)`
  - Processa tutti gli eventi pendenti (tastiera, mouse, finestra). Va chiamato una volta per frame.
- `void glfwSwapBuffers(GLFWwindow *window)`
  - Scambia i buffer della finestra (double buffering) dopo aver renderizzato il frame.

## 4. input da tastiera e mouse
- `void glfwSetKeyCallback(GLFWwindow *window, GLFWkeyfun cbfun)`
  - Callback per tasti premuti, rilasciati o ripetuti. Perfetto per:
    - Traslazioni sull’asse X/Y/Z
    - Cambio modalità colore/texture
    - Toggle smooth shading / wireframe
- `void glfwSetCursorPosCallback(GLFWwindow *window, GLFWcursorposfun cbfun)`
  - Traccia l’orientamento della camera usando il mouse (pitch/yaw) se vuoi un controllo più naturale.
- `int glfwGetKey(GLFWwindow *window, int key)`
  - Query immediate dello stato di un tasto; utile per input continuo (es. movimento mentre la finestra riceve focus).
- `void glfwSetInputMode(GLFWwindow *window, int mode, int value)`
  - Permette di nascondere o bloccare il cursore (`GLFW_CURSOR_DISABLED`) durante il volo della camera.

## 5. tempo e animazioni
- `double glfwGetTime(void)`
  - Restituisce i secondi dal momento dell’inizializzazione. Usalo per:
    - Calcolare delta time tra frame
    - Aggiornare la rotazione dell’oggetto attorno all’asse centrale
    - Mostrare FPS (1 / delta)

## 6. callback aggiuntive
- `void glfwSetFramebufferSizeCallback(GLFWwindow *window, GLFWframebuffersizefun cbfun)`
  - Aggiorna `glViewport` quando la finestra cambia dimensione, così la proiezione prospettica resta corretta.
- `void glfwSetScrollCallback(GLFWwindow *window, GLFWscrollfun cbfun)`
  - Usa la rotella per zoom/scaling sull’oggetto.
- `void glfwSetMouseButtonCallback(GLFWwindow *window, GLFWmousebuttonfun cbfun)`
  - Gestisce click specifici (es. selezione o toggle interfaccia).

## 7. monitor, fullscreen e finestre multiple
- `GLFWmonitor **glfwGetMonitors(int *count)` / `GLFWmonitor *glfwGetPrimaryMonitor(void)`
  - Recuperano i monitor disponibili. Puoi passare un monitor a `glfwCreateWindow` per fullscreen.
- `const GLFWvidmode *glfwGetVideoMode(GLFWmonitor *monitor)`
  - Restituisce risoluzione/refresh del monitor: utile per impostare correttamente un fullscreen mode.

## 8. joystick e gamepad (opzionale)
- `int glfwJoystickPresent(int jid)` e `const float *glfwGetJoystickAxes(int jid, int *count)`
  - Permettono di aggiungere movimento con gamepad (bonus o controlli alternativi).

## 9. terminazione e pulizia
- `void glfwSetWindowShouldClose(GLFWwindow *window, int value)`
  - Puoi settare il flag di chiusura dall’interno di un callback (es. tasto ESC).
- `void glfwDestroyWindow(GLFWwindow *window)`
  - Chiama questa funzione quando la finestra non serve più (prima di `glfwTerminate`).

## suggerimenti per SCOP
1. **Pipeline**: `glfwInit` → window hints → `glfwCreateWindow` → `glfwMakeContextCurrent` → caricare le funzioni GL (GLAD o altro) → configurare callback → loop (`glfwPollEvents`, render, `glfwSwapBuffers`).
2. **Input**: usa `glfwSetKeyCallback` per toggle texture/colore, cambio shading, reset rotazione. Per movimenti fluidi combina callback e `glfwGetKey` nel loop.
3. **Delta time**: misura `double now = glfwGetTime(); dt = now - last;` per ruotare senza dipendere dagli FPS.
4. **Multi-window/debug**: puoi aprire una finestra secondaria per visualizzare HUD o buffer di debug se necessario, riutilizzando le stesse funzioni.
5. **Requisiti**: con GLFW gestisci solo finestra ed eventi (consentito). Parser OBJ, matrici e shader restano sviluppati internamente come richiesto.

Con questo riferimento puoi mappare rapidamente ogni requisito del soggetto SCOP alle chiamate GLFW corrispondenti, mantenendo il progetto entro i vincoli (nessuna libreria esterna per geometria, solo window+input).
