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
        String [] salas = new String[20];
        String [] horas = new String[20];
        //salas
        String [][] sala1M = new String[30][10];
        String [][] sala1T = new String[30][10];
        String [][] sala2M = new String[30][10];
        String [][] sala2T = new String[30][10];
        String [][] sala3M = new String[30][10];
        String [][] sala3T = new String[30][10];

        int cantidadClientes = lecturaClientes(nombres, apellidos, ruts, contraseñas, saldos);
        lecturaStatus(ruts, estados, cantidadClientes);
        //int cantidadPeliculas = lecturaPeliculas(peliculas, tipos, recaudaciones, salas, horas);
        inicioSesion(ruts, contraseñas, nombres, apellidos, saldos, cantidadClientes);
    }


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
        }
      }
      arch.close();
      

    }

    public static int lecturaPeliculas(String [] peliculas, String [] tipos, int [] recaudaciones, String [] salas, String [] horas)throws IOException{
      int cant = 0;
      File file = new File("C:/Users/ignac/Desktop/   /UCN/Segundo año/Segundo semestre/Prog. Avanzada/Taller 0/Código/peliculas.txt");
      Scanner arch = new Scanner(file);
      while(arch.hasNextLine()){
        String linea = arch.nextLine();
        String [] partes = linea.split(",");
        String pelicula = partes[0];
        String tipo = partes[1];
        int recaudacion = Integer.parseInt(partes[2]);
        peliculas[cant] = pelicula;
        tipos[cant]= tipo;
        recaudaciones[cant] = recaudacion;
        for(int i = 3;i<partes.length; i+=2){
          String sala = partes[i];
          String hora = partes[i+1];
          salas[cant] = sala;
          horas[cant] = hora;
          
          }
          
        
        for(int i = 0; i<cant;i++){
          if(salas[i] != null){ 
          System.out.println(salas[i]+horas[i]+"\n");
          }
        }
        
      }
      arch.close();
      return cant;  
    }

    public static void inicioSesion(String [] ruts,String [] contraseñas, String [] nombres, String [] apellidos, int [] saldos, int cantidadClientes){
      Scanner scan = new Scanner(System.in);
      int cant = 0;
      System.out.println("Bienvenido al menu de usuario, ingrese 0 para cerrar el sistema.");
      while(true){ 
        System.out.println("Ingrese su rut: ");
        String rut = scan.nextLine();
        String rutF = FormateoRut(rut);
        System.out.println(rutF);
        int indexRut = index(ruts, cantidadClientes, rutF);
        if(indexRut != -1){
          System.out.println("Ingrese su contraseña: ");
          String contraseña = scan.nextLine();
          int indexPass = index(contraseñas, cantidadClientes, contraseña);
          if(indexPass == indexRut){
            //menuCliente(peliculas, rut, tipos, salas, horas, sala1M, sala1T, sala2M, sala2T, sala3M, sala3T, cantidadPeliculas);

         }else{
            System.out.println("contraseña incorrecta, desea volver a intentarlo?\n[1] Si.\n[2] No.");
            int opcion = Integer.parseInt(scan.nextLine());
            if(opcion == 2){
              break;
            }
          }
        }if(indexRut == -1){
          agregarCliente(ruts, contraseñas, saldos, nombres, apellidos, cantidadClientes);
        
      }if(rutF.equals("0")){
        System.out.println("Cierre del sistema.");
          break;
      }if(rutF.equals("ADMIN")){
        System.out.println("Ingrese su contraseña: ");
        String contraseñaA = scan.nextLine();
        if(contraseñaA.equals("ADMIN")){

        }
      }
    }

    }

    public static void agregarCliente(String [] ruts, String [] contraseñas, int [] saldos, String [] nombres, String [] apellidos, int cantidadClientes){
      Scanner scan = new Scanner(System.in);
      
      System.out.println("El rut no se encuentra en el sistema, desea registrarse?\n[1] Si.\n[2] No.");
      int opcion = Integer.parseInt(scan.nextLine());
      if(opcion == 2){
        System.out.println("Se cierra lonji.");
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


    public static void menuCliente(String [] peliculas, String rut ,String [] tipos, String [] salas, String [] horas, String [][] sala1M, 
    String [][] sala1T, String [][] sala2M, String [][] sala2T, String [][] sala3M, String [][] sala3T, int cantidadPeliculas){
      Scanner scan = new Scanner(System.in);
      System.out.println("Bienvenido al menu del cliente, eliga una opcion\n[1] Comprar entrada.\n[2] Informacion del usuario.\n[3] Devolucion entrada.\n[4] Cartelera.");
      int opcion = Integer.parseInt(scan.nextLine());
      if(opcion == 1){
        comprarEntrada(peliculas, tipos, salas, horas, sala1M, sala1T, sala2M, sala2T, sala3M, sala3T, cantidadPeliculas);
      }
      if(opcion == 2){

      }
      if(opcion == 3){

      }
      if(opcion == 4){
        cartelera(peliculas, horas, cantidadPeliculas);
      }
      
    }

    public static void comprarEntrada(String [] peliculas, String [] tipos, String [] salas, String [] horas, String [][] sala1M, 
    String [][] sala1T, String [][] sala2M, String [][] sala2T, String [][] sala3M, String [][] sala3T, int cantidadPeliculas){
      Scanner scan = new Scanner(System.in);
      System.out.println("Ingrese el nombre de la pelicula que desea comprar: ");
      String pelicula = scan.nextLine();
      int indexPelicula = index(peliculas, cantidadPeliculas, pelicula);
      if(indexPelicula != -1){
        System.out.println("Los horarios de la pelicula "+peliculas[indexPelicula]+" son: ");
        for(int i = indexPelicula;i<cantidadPeliculas;i++){
          System.out.println(salas[indexPelicula]+"Con hora en "+horas[indexPelicula]+"\n");
        }
        System.out.println("Seleccione la sala: ");
        String sala = scan.nextLine();
        System.out.println("Seleccione la hora: ");
        String hora = scan.nextLine();
        
        
      }

    }

    public static void infoUsuario(String [] ruts, String rut ,String [] nombres, String [] apellidos, String [] saldo, String [] horas ){
      
    }

    public static void devolucion(){

    }

    public static void cartelera(String [] peliculas, String [] horas, int cantidadPeliculas){
      for(int i = 0;i<cantidadPeliculas;i++){
        System.out.println("Pelicula "+peliculas[i]+" y su horario es en la: "+horas[i]+"\n");
      }

    }

    public static void menuAdmin(String [] peliculas, String [] recaudaciones, String [] ruts, String [] nombres, String [] apellidos,
    String [] saldos, int cantidadClientes){
      Scanner scan = new Scanner(System.in);
      System.out.println("Eliga una opcion\n[1] Taquilla.\n[2] Informacion cliente.");
      int opcion = Integer.parseInt(scan.nextLine());
      if(opcion == 1){

      }if(opcion == 2){

      }

    }

    public static void taquilla(String [] peliculas, String [] recaudaciones){

    }

    public static void infoCliente(String [] ruts, String [] nombres, String [] apellidos, String [] saldos, int cantidadClientes){
      Scanner scan = new Scanner(System.in);
      System.out.println("Ingrese el rut del cliente: ");
      String rut = scan.nextLine();
      int indexRut = index(ruts, cantidadClientes, rut);
      if(indexRut != -1){
        System.out.println("Nombre: "+nombres[indexRut]+" Apellido: "+apellidos[indexRut]+" Saldo: "+saldos[indexRut]);
      }

    }


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


  public static int index(String [] lista, int cant, String dato){
    for(int i=0; i<cant;i++){
      if(lista[i].equals(dato)){
        return i;
      }      
      }
      return -1;
    }

}
