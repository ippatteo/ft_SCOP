# GLFW consentita in SCOP: funzioni essenziali

Il confine del subject e corretto: in SCOP e consentita la gestione di finestra, eventi e contesto OpenGL, mentre restano da implementare internamente matematica 3D, shader e caricamento asset.

La buona notizia e questa: puoi usare tecnicamente tutta GLFW, perche e una libreria pensata proprio per finestra, input, eventi e contesto OpenGL. Non fornisce utility per matrici 3D, parser OBJ o compilazione shader ad alto livello.

Di seguito trovi solo le funzioni fondamentali e sicure da usare nel progetto.

## 1. Inizializzazione e Terminazione

- `glfwInit()`
  - Inizializza la libreria GLFW. Va chiamata all'avvio.
- `glfwTerminate()`
  - Rilascia le risorse GLFW e chiude correttamente il framework.

## 2. Gestione della Finestra e del Contesto

- `glfwWindowHint()`
  - Imposta i parametri del contesto prima della creazione della finestra (esempio: versione OpenGL e profilo Core).
- `glfwCreateWindow()`
  - Crea la finestra principale.
- `glfwDestroyWindow()`
  - Distrugge la finestra quando non serve piu.
- `glfwMakeContextCurrent()`
  - Associa il contesto OpenGL alla finestra corrente.
- `glfwSwapBuffers()`
  - Scambia front/back buffer dopo il rendering del frame.
- `glfwWindowShouldClose()`
  - Verifica se la finestra deve essere chiusa (ad esempio click su X).
- `glfwSetWindowShouldClose()`
  - Imposta la chiusura programmatica della finestra (esempio: tasto ESC).

## 3. Gestione Eventi e Loop

- `glfwPollEvents()`
  - Elabora gli eventi pendenti (tastiera, mouse, finestra). Va chiamata nel loop principale.

## 4. Input (Tastiera e Mouse)

- `glfwGetKey()`
  - Legge lo stato corrente di un tasto. Utile per movimento continuo.
- `glfwSetKeyCallback()`
  - Registra callback per pressione/rilascio tasti. Utile per toggle (esempio texture on/off).
- `glfwSetCursorPosCallback()`
  - Callback per il movimento del mouse.
- `glfwGetCursorPos()`
  - Legge la posizione corrente del cursore.

## 5. Gestione del Tempo

- `glfwGetTime()`
  - Restituisce i secondi trascorsi dall'avvio di GLFW. Base per delta time e animazioni indipendenti dagli FPS.

## 6. Funzione Extra Utile

- `glfwSetFramebufferSizeCallback()`
  - Callback invocata al resize della finestra. Serve per aggiornare `glViewport` e mantenere la proiezione corretta.

## Nota pratica per SCOP

- Con queste API resti nel perimetro consentito dal subject.
- Parser OBJ, algebra lineare (vettori/matrici), camera e shader management restano implementati da te.
