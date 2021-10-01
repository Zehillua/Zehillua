import java.io.BufferedReader;
import java.io.File;
import java.io.FileReader;
import java.io.IOException;
import java.util.Scanner;

public class Taller0{

    public static void main(String[] args)throws IOException {
        
        //clientes.txt
        String [] nombres = new String [1000];
        String [] apellidos = new String[1000];
        String [] ruts = new String[1000];
        String [] contraseñas = new String[1000];
        int [] saldos = new int[1000];
        //status.txt
        String [] estados = new String[1000];
        //peliculas.txt
        String [] peliculas = new String[20];
        String [] tipos = new String[20];
        int [] recaudaciones = new int[20];
        boolean funcionesM [][] = new boolean[500][3];
        boolean funcionesT [][] = new boolean[500][3];
        //salas
        String [][] sala1M = new String[30][10];
        String [][] sala1T = new String[30][10];
        String [][] sala2M = new String[30][10];
        String [][] sala2T = new String[30][10];
        String [][] sala3M = new String[30][10];
        String [][] sala3T = new String[30][10];

        int cantidadClientes = lecturaClientes(nombres, apellidos, ruts, contraseñas, saldos);
        lecturaStatus(ruts, estados, cantidadClientes);
        int cantidadPeliculas = lecturaPeliculas(peliculas, tipos, recaudaciones,funcionesM, funcionesT);
        lecturaFunciones(funcionesM, funcionesT, peliculas, cantidadPeliculas);
        inicioSesion(ruts, contraseñas, nombres, apellidos, saldos, cantidadClientes, peliculas, tipos, sala1M, 
        sala1T,sala2M,sala2T, sala3M,sala3T,cantidadPeliculas);
    }

    /**
     * 
     * @param nombres will be to save the names in the nombres list.
     * @param apellidos will be to save the lastnames in the apellidos list.
     * @param ruts will be to save the ruts in the ruts list.
     * @param contraseñas will be to save the passwords in the contraseñas list.
     * @param saldos will be to save the balances in the saldos list.
     * @return to variable cant
     * @throws IOException It will serve to read the file of clientes.txt
     */
    public static int lecturaClientes(String [] nombres, String[] apellidos,String[] ruts,String [] contraseñas, int[] saldos)throws IOException {
        int cant = 0;
        File file = new File("C:/Users/ignac/Desktop/   /UCN/Segundo año/Segundo semestre/Prog. Avanzada/Taller 0/Código/clientes.txt");
        Scanner arch = new Scanner(file);
        while(arch.hasNextLine()){
          String linea = arch.nextLine();
          String [] datos = linea.split(",");
          String nombre = datos[0];
          String apellido = datos[1];
          String rut = datos[2];
          String contraseña = datos[3];
          int saldo = Integer.parseInt(datos[4]);
          String rutF = FormateoRut(rut);
          nombres[cant] = nombre;
          apellidos[cant] = apellido;
          ruts[cant] = rutF;
          contraseñas[cant]=contraseña;
          saldos[cant]=saldo;
          cant++;
        }
        arch.close();
        return cant;
        
    }

    /**
     * 
     * @param ruts will be to save the ruts in the ruts list.
     * @param estados will be to save the status in the estados list.
     * @param cantidadClientes It will be used to read up to the number of people entered and not the entire length of the ruts list
     * @throws IOException It will serve to read the file of status.txt
     */
    public static void lecturaStatus (String [] ruts, String [] estados, int cantidadClientes)throws IOException{
      int cant = 0;
      File file = new File("C:/Users/ignac/Desktop/   /UCN/Segundo año/Segundo semestre/Prog. Avanzada/Taller 0/Código/status.txt");
      Scanner arch = new Scanner(file);
      while(arch.hasNextLine()){
        String linea = arch.nextLine();
        String [] partes = linea.split(",");
        String rut = partes[0];
        String rutF = FormateoRut(rut);
        int indexRut = index(ruts, cantidadClientes , rutF);
        if(indexRut != -1){
          String estado = partes[1];
          estados[cant] = estado;
        }
        cant++;
      }
      arch.close();
      

    }

    /**
     * 
     * @param peliculas will be to save the movies in the peliculas list.
     * @param tipos will be to save the types in the tipos list.
     * @param recaudaciones will be to save the recaudations in the recaudaciones list.
     * @param funcionesM will be to save the funcions in the morning in the funcionesM matriz.
     * @param funcionesT will be to save the functions in the afternoon in the funcionesT matriz.
     * @return to variable cant.
     * @throws IOException It will serve to read the file of peliculas.txt
     */

    public static int lecturaPeliculas(String [] peliculas, String [] tipos, int [] recaudaciones, boolean [][] funcionesM, boolean [][] funcionesT)throws IOException{
      int cant = 0;
      File file = new File("C:/Users/ignac/Desktop/   /UCN/Segundo año/Segundo semestre/Prog. Avanzada/Taller 0/Código/peliculas.txt");
      Scanner arch = new Scanner(file);
      for(int i = 0;i<=cant;i++){
        for(int j = 0;j<=2;j++){
          funcionesM[i][j] = false;
          funcionesT[i][j] = false;
        }
      }
      while(arch.hasNextLine()){
        String linea = arch.nextLine();
        String [] partes = linea.split(",");
        String pelicula = partes[0];
        String tipo = partes[1];
        int recaudacion = Integer.parseInt(partes[2]);
        peliculas[cant] = pelicula;
        tipos[cant]= tipo;
        recaudaciones[cant] = recaudacion;
        cant++;
      }  
      arch.close();
      return cant;  
    }

    /**
     * 
     * @param funcionesM will be to save the funcions in the morning in the funcionesM matriz.
     * @param funcionesT will be to save the functions in the afternoon in the funcionesT matriz.
     * @param peliculas  It will be used to connect the movie with its functions.
     * @param cantidadPeliculas It will be used to search by the number of movies and not by the length of the list.
     * @throws IOException It will serve to read the file of peliculas.txt
     */

    public static void lecturaFunciones(boolean [][] funcionesM, boolean [][] funcionesT, String [] peliculas, int cantidadPeliculas)throws IOException{
      File file = new File("C:/Users/ignac/Desktop/   /UCN/Segundo año/Segundo semestre/Prog. Avanzada/Taller 0/Código/peliculas.txt");
      Scanner arch = new Scanner(file);
      for(int i =0;i<cantidadPeliculas;i++){
        for(int j=0;j<=2;j++){
          funcionesM[i][j]=false;
          funcionesT[i][j]=false;
        }
      }
      while(arch.hasNextLine()){
        String linea = arch.nextLine();
        String [] partes = linea.split(",");
        String pelicula = partes[0];
        int indexPelicula = index(peliculas, cantidadPeliculas, pelicula);
        if(indexPelicula !=-1){
          for(int i =3;i<partes.length;i+=2){
            String sala = partes[i];
            String hora = partes[i+1];
            if(hora.equals("M")){
              if(sala.equals("1")){
                funcionesM[indexPelicula][0] = true;
              }
              if(sala.equals("2")){
                funcionesM[indexPelicula][1] = true;
              }
              if(sala.equals("3")){
                funcionesM[indexPelicula][2] = true;
              }
            }
            if(hora.equals("T")){
              if(sala.equals("1")){
                funcionesT[indexPelicula][0] = true;
              }
              if(sala.equals("2")){
                funcionesT[indexPelicula][1] = true;
              }
              if(sala.equals("3")){
                funcionesT[indexPelicula][2] = true;
              }
            }
          }
        }
      }
      
      arch.close();
    }
   
    /**
     * 
     * @param ruts It will be used to search the list if the rut to start session is registered.
     * @param contraseñas It will be used to see if the password matches the client's rut.
     * @param nombres It will be used to search for the customer's name in the nombres list
     * @param apellidos It will be used to search for the customer's lastname in the apellidos list
     * @param saldos It will be used to search for the customer's balance in the saldos list
     * @param cantidadClientes It will be used to search by the number of clients and not by the length of the list.
     * @param peliculas It will be used to search for the movie in the peliculas list.
     * @param tipos It will be used to search for the type movie in the tipos list
     * @param sala1M It is a room for the movies that will be shown in room 1 in the morning.
     * @param sala1T It is a room for the movies that will be shown in room 1 in the afternoon.
     * @param sala2M It is a room for the movies that will be shown in room 2 in the morning
     * @param sala2T It is a room for the movies that will be shown in room 2 in the afternoon.
     * @param sala3M It is a room for the movies that will be shown in room 3 in the morning
     * @param sala3T It is a room for the movies that will be shown in room 3 in the afternoon.
     * @param cantidadPeliculas It will be used to search by the number of movies and not by the length of the list.
     */   

    public static void inicioSesion(String [] ruts,String [] contraseñas, String [] nombres, String [] apellidos, int [] saldos, int cantidadClientes,
    String [] peliculas, String [] tipos, String [][] sala1M, 
    String [][] sala1T, String [][] sala2M, String [][] sala2T, String [][] sala3M, String [][] sala3T, int cantidadPeliculas){
      Scanner scan = new Scanner(System.in);
      int cant = 0;
      System.out.println("Bienvenido al menu de usuario, ingrese 0 para cerrar el sistema.");
      while(true){ 
        System.out.println("Ingrese su rut: ");
        String rut = scan.nextLine();
        if(rut.equals("0")){
          System.out.println("Cierre del sistema.");
            break;
        }
        String rutF = FormateoRut(rut);
        int indexRut = index(ruts, cantidadClientes, rutF);
        if(indexRut != -1){
          System.out.println("Ingrese su contraseña: ");
          String contraseña = scan.nextLine();
          int indexPass = index(contraseñas, cantidadClientes, contraseña);
          if(indexPass == indexRut){
            menuCliente(peliculas, rutF, tipos, sala1M, sala1T, sala2M, sala2T, sala3M, sala3T, cantidadPeliculas, ruts, nombres, apellidos, saldos, cantidadClientes);

         }else{
            System.out.println("contraseña incorrecta, desea volver a intentarlo?\n[1] Si.\n[2] No.");
            int opcion = Integer.parseInt(scan.nextLine());
            if(opcion == 2){
              break;
            }
          }
        }if(indexRut == -1){
          agregarCliente(ruts, contraseñas, saldos, nombres, apellidos, cantidadClientes);
        
        }
        if(rutF.equals("ADMIN")){
          System.out.println("Ingrese su contraseña: ");
          String contraseñaA = scan.nextLine();
          if(contraseñaA.equals("ADMIN")){

          }else{
            System.out.println("Contraseña incorrecta.");
          }
        }
      }

    }

    /**
     * 
     * @param ruts will be used to add the new rut to the ruts list.
     * @param contraseñas will be used to add the new password to the contraseñas list.
     * @param saldos will be used to add the new balance to the saldos list.
     * @param nombres will be used to add the new name to the nombres list.
     * @param apellidos will be used to add the new lastname to the apellidos list.
     * @param cantidadClientes It will be used to search by the number of clients and not by the length of the list.
     */

    public static void agregarCliente(String [] ruts, String [] contraseñas, int [] saldos, String [] nombres, String [] apellidos, int cantidadClientes){
      Scanner scan = new Scanner(System.in);
      
      System.out.println("El rut no se encuentra en el sistema, desea registrarse?\n[1] Si.\n[2] No.");
      int opcion = Integer.parseInt(scan.nextLine());
      if(opcion == 2){
        System.out.println("Se cierra el sistema.");
      }else{
        System.out.println("Ingrese su rut: ");
        String rut = scan.nextLine();
        String rutF = FormateoRut(rut);
        System.out.println("Ingrese su contraseña: ");
        String contraseña = scan.nextLine();
        System.out.println("Ingrese su nombre: ");
        String nombre = scan.nextLine();
        System.out.println("Ingrese su apellido: ");
        String apellido = scan.nextLine();
        System.out.println("Ingrese su saldo: ");
        int saldo = Integer.parseInt(scan.nextLine());
        System.out.println(rutF);
        ruts[cantidadClientes] = rutF;
        contraseñas[cantidadClientes] =contraseña;
        nombres[cantidadClientes] = nombre;
        apellidos[cantidadClientes] = apellido;
        saldos[cantidadClientes] = saldo;
        cantidadClientes++;        
        System.out.println("A sido registrado correctamente.");
      }
      
      
      
    }

    /**
     * 
     * @param peliculas It will be used to search for the movie in the peliculas list.
     * @param rutF Is the new rut without dots and dash.
     * @param tipos It will be used to search for the movie type in the tipos list.
     * @param sala1M It will be used for when they want to compare a seat in room 1 in the morning.
     * @param sala1T It will be used for when they want to compare a seat in room 1 in the afternoon.
     * @param sala2M It will be used for when they want to compare a seat in room 2 in the morning
     * @param sala2T It will be used for when they want to compare a seat in room 2 in the afternoon.
     * @param sala3M It will be used for when they want to compare a seat in room 3 in the morning
     * @param sala3T It will be used for when they want to compare a seat in room 3 in the afternoon.
     * @param cantidadPeliculas It will be used to search by the number of movies and not by the length of the list.
     * @param ruts It will be used to search for the rut in the ruts list.
     * @param nombres It will be used to search for the name in the nombres list.
     * @param apellidos It will be used to search for the lastname in the apellidos list.
     * @param saldos It will be used to search for the balance in the saldos list.
     * @param cantidadClientes It will be used to search by the number of clients and not by the length of the list.
     */

    public static void menuCliente(String [] peliculas, String rutF ,String [] tipos, String [][] sala1M, 
    String [][] sala1T, String [][] sala2M, String [][] sala2T, String [][] sala3M, String [][] sala3T, int cantidadPeliculas,
    String [] ruts, String [] nombres, String [] apellidos, int [] saldos, int cantidadClientes){
      Scanner scan = new Scanner(System.in);
      System.out.println("Bienvenido al menu del cliente, eliga una opcion\n[1] Comprar entrada.\n[2] Informacion del usuario.\n[3] Devolucion entrada.\n[4] Cartelera.");
      int opcion = Integer.parseInt(scan.nextLine());
      if(opcion == 1){
        comprarEntrada(peliculas, tipos,sala1M, sala1T, sala2M, sala2T, sala3M, sala3T, cantidadPeliculas);
      }
      if(opcion == 2){
        infoUsuario(ruts, rutF, nombres, apellidos, saldos,cantidadClientes);
      }
      if(opcion == 3){
        devolucion();
      }
      if(opcion == 4){
        cartelera(peliculas,cantidadPeliculas);
      }
      
    }


    /**
     * 
     * @param peliculas It will be used to search for the movie that the client want in the peliculas list.
     * @param tipos It will be used to indicate to the client if it is a movie in premiere or released.
     * @param sala1M It will be used for when they want to compare a seat in room 1 in the morning.
     * @param sala1T It will be used for when they want to compare a seat in room 1 in the afternoon.
     * @param sala2M It will be used for when they want to compare a seat in room 2 in the morning.
     * @param sala2T It will be used for when they want to compare a seat in room 2 in the afternoon.
     * @param sala3M It will be used for when they want to compare a seat in room 3 in the morning.
     * @param sala3T It will be used for when they want to compare a seat in room 3 in the afternoon.
     * @param cantidadPeliculas It will be used to search by the number of movies and not by the length of the list.
     */

    public static void comprarEntrada(String [] peliculas, String [] tipos, String [][] sala1M, 
    String [][] sala1T, String [][] sala2M, String [][] sala2T, String [][] sala3M, String [][] sala3T, int cantidadPeliculas){
      Scanner scan = new Scanner(System.in);
      System.out.println("Ingrese el nombre de la pelicula que desea comprar: ");
      String pelicula = scan.nextLine();
      int indexPelicula = index(peliculas, cantidadPeliculas, pelicula);
      if(indexPelicula != -1){
        System.out.println("Los horarios de la pelicula "+peliculas[indexPelicula]+" son: ");
        for(int i = indexPelicula;i<cantidadPeliculas;i++){
          System.out.println("");
        }
        System.out.println("Seleccione la sala: ");
        String sala = scan.nextLine();
        System.out.println("Seleccione la hora: ");
        String hora = scan.nextLine();
        
        
      }

    }

    /**
     * 
     * @param ruts It will be used to search for the rut in the ruts list.
     * @param rutF Is the new rut without dots and dash.
     * @param nombres It will be used to search for the name in the nombres list.
     * @param apellidos It will be used to search for the lastname in the apellidos list.
     * @param saldos It will be used to search for the balance in the saldos list.
     * @param cantidadClientes It will be used to search by the number of clients and not by the length of the list.
     */

    public static void infoUsuario(String [] ruts, String rutF ,String [] nombres, String [] apellidos, int [] saldos,int cantidadClientes){
      int indexRut = index(ruts, cantidadClientes, rutF);
      if(indexRut !=-1){
        String rutN = pointRut(ruts[indexRut]);
        System.out.println("Rut: "+rutN+" Nombre: "+nombres[indexRut]+" Apellido: "+apellidos[indexRut]+" Saldo: "+saldos[indexRut]);
    }
  }

    public static void devolucion(){

    }

    /**
     * 
     * @param peliculas It will be used to search for the movie in the peliculas list.
     * @param cantidadPeliculas It will be used to search by the number of movies and not by the length of the list.
     */

    public static void cartelera(String [] peliculas, int cantidadPeliculas){
      for(int i = 0;i<cantidadPeliculas;i++){
        System.out.println("Pelicula "+peliculas[i]);
      }

    }

    /**
     * 
     * @param peliculas It will be used to search for the movie in the peliculas list.
     * @param recaudaciones will be to save the recaudations in the recaudaciones list.
     * @param ruts It will be used to search for the rut in the ruts list.
     * @param nombres It will be used to search for the name in the nombres list.
     * @param apellidos It will be used to search for the lastname in the apellidos list.
     * @param saldos It will be used to search for the balance in the saldos list.
     * @param cantidadClientes It will be used to search by the number of clients and not by the length of the list.
     */

    public static void menuAdmin(String [] peliculas, String [] recaudaciones, String [] ruts, String [] nombres, String [] apellidos,
    String [] saldos, int cantidadClientes){
      Scanner scan = new Scanner(System.in);
      System.out.println("Eliga una opcion\n[1] Taquilla.\n[2] Informacion cliente.");
      int opcion = Integer.parseInt(scan.nextLine());
      if(opcion == 1){
        taquilla(peliculas, recaudaciones);
      }if(opcion == 2){
        infoCliente(ruts, nombres, apellidos, saldos, cantidadClientes);
      }

    }

    /**
     * 
     * @param peliculas It will be used to search for the movie in the peliculas list.
     * @param recaudaciones will be to save the recaudations in the recaudaciones list.
     */

    public static void taquilla(String [] peliculas, String [] recaudaciones){

    }

    /**
     * 
     * @param ruts It will be used to search for the rut in the ruts list.
     * @param nombres It will be used to search for the name in the nombres list.
     * @param apellidos It will be used to search for the lastname in the apellidos list.
     * @param saldos It will be used to search for the balance in the saldos list.
     * @param cantidadClientes It will be used to search by the number of clients and not by the length of the list.
     */

    public static void infoCliente(String [] ruts, String [] nombres, String [] apellidos, String [] saldos, int cantidadClientes){
      Scanner scan = new Scanner(System.in);
      System.out.println("Ingrese el rut del cliente: ");
      String rut = scan.nextLine();
      int indexRut = index(ruts, cantidadClientes, rut);
      if(indexRut != -1){
        System.out.println("Nombre: "+nombres[indexRut]+" Apellido: "+apellidos[indexRut]+" Saldo: "+saldos[indexRut]);
      }

    }

    /**
     * 
     * @param rut It will be used to remove the points and the hyphen from the read rut.
     * @return to variable cont.
     */

    public static String FormateoRut(String rut) {
      int cont=0;
      String format;
      rut=rut.replace(".", "");
      rut=rut.replace("-", "");
      format = "-"+rut.substring(rut.length()-1);
      for(int i = rut.length()-2;i>=0;i--){
          format=rut.substring(i, i+1)+format;
          cont++;
          if(cont == 3 && i!=0){
              format="."+format;
              cont=0;
          }
      }
  
      return rut.toLowerCase(); 
  }
  
  /**
   * 
   * @param rut It will be used to put the points and the hyphen to the read routine.
   * @return to variable cont.
   */

  public static String pointRut(String rut) {
      int cont=0;
      String format;
      rut=rut.replace(",", "");
      rut=rut.replace("-", "");
      format = "-"+rut.substring(rut.length()-1);
      for(int i = rut.length()-2;i>=0;i--){
          format=rut.substring(i, i+1)+format;
          cont++;
          if(cont == 3 && i!=0){
              format="."+format;
              cont=0;
          }
      }
  
      return format; 
  }

  /**
   * 
   * @param lista will be the list in which it will be searched.
   * @param cant will be the amount that will have to travel to find the index.
   * @param dato It will be the data that will be searched in its respective list.
   * @return to -1.
   */

  public static int index(String [] lista, int cant, String dato){
    for(int i=0; i<cant;i++){
      if(lista[i].equals(dato)){
        return i;
      }      
      }
      return -1;
    }

}
