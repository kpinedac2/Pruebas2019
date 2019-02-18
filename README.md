 BufferedReader br = new BufferedReader (new InputStreamReader (System.in));
        System.out.println ("Introduzca un Numero: ");
        String nu = br.readLine ();
        int n = Integer.parseInt (nu);
        int suma=0;
        for (int i = 1 ; i <= n ; i++){
            if (n % i == 0){
             if(i==1)
             {  
            out.print("");
             }else{
                if(esPrimo(i))
                {
                out.println(i+" Es Primo");
                suma+=i;
               }
             }                 
            }
        }
        out.println("Suma Total de numeros Primos "+suma);        
    }
     public static boolean esPrimo(int numero){
      int contador = 2;
      boolean primo=true;
      while ((primo) && (contador!=numero)){
        if (numero % contador == 0)
          primo = false;
        contador++;
      }
      return primo; 
    }
     
}


-------------------------------



        int rango[] ={1,9};
        algo(20,4,rango);
                 
}
        
           
public static void algo(int suma,int sumandos, int rango[]){
       
        ArrayList<Integer> _rango=new ArrayList<Integer>();//se almacena rango de numeros
        ArrayList<Object> _sumandos=new ArrayList<Object>();

            //llenar lista con el rango 
        for(int i=rango[1]; i<=rango[1];i++)
        {
            _rango.add(i);
        }
        
        //llenamos una lista con los rangos que representaran a los sumandos
        for(int i=0; i<suma;i++)
        {
            
        _sumandos.add(_rango);
        
        }
        
        
        //sumamos ....
        int siguiente=0;
        for(int i=0;i<sumandos;i++)
        {
            
//            out.println(_sumandos.get(i));
            ArrayList<Integer> lista=(ArrayList<Integer>)_sumandos.get(i);//recorremos las listas que estan dentro de la  lista
            
           for(int j=0;j<lista.size();j++) 
           {
             
           //out.print(lista.get(j));
             out.print(" + ");
             
            
                 for(int k=sumandos; k<lista.size(); k++)
                 {
                     out.print(lista.get(k));
                      out.println(k);
                 }
                 
           }
            
         
       
        }
    }
           
           
      
  
    }



--------------------------
CREATE DATABASE kervins_test;

USE kervins_test;

CREATE TABLE usuario (
	id_usuario int identity(1,1) not null PRIMARY KEY,
	username VARCHAR(25) UNIQUE
);

CREATE TABLE roles (
	id_rol int identity(1,1) PRIMARY KEY,
	nombre_rol VARCHAR(25) UNIQUE
);

CREATE TABLE usuario_rol (
	id int identity(1,1) PRIMARY KEY,
	id_usuario int,
    id_rol int,
    CONSTRAINT Fk_usuario_asig FOREIGN KEY (id_usuario) REFERENCES usuario (id_usuario),
    CONSTRAINT Fk_rol_asig FOREIGN KEY (id_rol) REFERENCES roles (id_rol)
);

CREATE TABLE modulos (
	id_modulo int identity(1,1) PRIMARY KEY,
	modulo VARCHAR(25) UNIQUE
);

CREATE TABLE permisos (
	id_permiso int identity(1,1) PRIMARY KEY,
	permiso VARCHAR(25),
    id_modulo int,
    CONSTRAINT Fk_modulo_permisos FOREIGN KEY (id_modulo) REFERENCES modulos (id_modulo)
);

CREATE TABLE rol_permisos (
	id int identity(1,1) PRIMARY KEY,
	id_permiso int,
    id_rol int,
    CONSTRAINT Fk_rols_asig FOREIGN KEY (id_rol) REFERENCES roles (id_rol)
);

INSERT INTO usuario (username)VALUES ('kervins1');
INSERT INTO usuario (username)VALUES ('kervins2');
INSERT INTO usuario (username)VALUES ('kervins3');

INSERT INTO roles (nombre_rol) VALUES ('VENDEDOR');
INSERT INTO roles (nombre_rol) VALUES ('ADMIN');

INSERT INTO usuario_rol(id_usuario, id_rol) VALUES ('1', '1');
INSERT INTO usuario_rol (id_usuario, id_rol) VALUES ('2', '2');
INSERT INTO usuario_rol (id_usuario, id_rol) VALUES ('3', '1');

INSERT INTO modulos(modulo) VALUES ('ADMON');
INSERT INTO modulos (modulo) VALUES ('VENTAS');
INSERT INTO modulos(modulo) VALUES ('A');


INSERT INTO permisos (permiso, id_modulo) VALUES ('LECTURA', '1');
INSERT INTO permisos(permiso, id_modulo) VALUES ('ESCRITURA', '1');
INSERT INTO permisos (permiso, id_modulo) VALUES ('TOTAL', '1');
INSERT INTO permisos (permiso, id_modulo) VALUES ('LECTURA', '2');
INSERT INTO permisos (permiso, id_modulo) VALUES ('ESCRITURA', '2');
INSERT INTO permisos (permiso, id_modulo) VALUES ('TOTAL', '2');
INSERT INTO permisos (permiso, id_modulo) VALUES ('SUPER', '1');

UPDATE permisos SET permiso = 'ESCRITURA_VENTAS' WHERE (id_permiso = '5');
UPDATE permisos SET permiso = 'LECTURA_ADMON' WHERE (id_permiso = '1');
UPDATE permisos SET permiso = 'ESCRITURA_ADMON' WHERE (id_permiso = '2');
UPDATE permisos SET permiso = 'TOTAL_ADMON' WHERE (id_permiso = '3');
UPDATE permisos SET permiso = 'LECTURA_VENTAS' WHERE (id_permiso = '4');
UPDATE permisos SET permiso = 'TOTAL_VENTAS' WHERE (id_permiso = '6');

INSERT INTO rol_permisos (id_permiso, id_rol) VALUES ('1', '2');
INSERT INTO rol_permisos (id_permiso, id_rol) VALUES ('2', '2');
INSERT INTO rol_permisos (id_permiso, id_rol) VALUES ('3', '2');
INSERT INTO rol_permisos (id_permiso, id_rol) VALUES ('4', '1');
INSERT INTO rol_permisos (id_permiso, id_rol) VALUES ('5', '1');
INSERT INTO rol_permisos (id_permiso, id_rol) VALUES ('6', '2');
INSERT INTO rol_permisos (id_permiso, id_rol) VALUES ('7', '2');
INSERT INTO rol_permisos (id_permiso, id_rol) VALUES ('5', '2');
INSERT INTO rol_permisos (id_permiso, id_rol) VALUES ('4', '2');

/* Permisos con usuarios */
SELECT u.username, p.permiso
FROM usuario as u
INNER JOIN usuario_rol as ur ON u.id_usuario = ur.id_usuario
INNER JOIN roles as r ON r.id_rol = ur.id_rol
INNER JOIN rol_permisos as rp ON rp.id_rol = r.id_rol
INNER JOIN permisos as p ON p.id_permiso = rp.id_permiso
ORDER BY u.username DESC

/* Contar cuantos permisos tienen */
SELECT u.username, COUNT(p.permiso)
FROM usuario as u
INNER JOIN usuario_rol as ur ON u.id_usuario = ur.id_usuario
INNER JOIN roles as r ON r.id_rol = ur.id_rol
INNER JOIN rol_permisos as rp ON rp.id_rol = r.id_rol
INNER JOIN permisos as p ON p.id_permiso = rp.id_permiso
GROUP BY u.username
ORDER BY u.username DESC

/* Saber cual si tiene cierto permiso */
SELECT DISTINCT u.username
FROM usuario as u
INNER JOIN usuario_rol as ur ON u.id_usuario = ur.id_usuario
INNER JOIN roles as r ON r.id_rol = ur.id_rol
INNER JOIN rol_permisos as rp ON rp.id_rol = r.id_rol
INNER JOIN permisos as p ON p.id_permiso = rp.id_permiso
WHERE P.permiso = 'SUPER'


/* Saber cual(es) no tienen  permiso */
SELECT u.username
FROM usuario as u
WHERE u.username NOT IN (
	SELECT DISTINCT u.username
FROM usuario as u
INNER JOIN usuario_rol as ur ON u.id_usuario = ur.id_usuario
INNER JOIN roles as r ON r.id_rol = ur.id_rol
INNER JOIN rol_permisos as rp ON rp.id_rol = r.id_rol
INNER JOIN permisos as p ON p.id_permiso = rp.id_permiso
inner join modulos as m on m.id_modulo=p.id_modulo
--WHERE P.permiso = 'SUPER'
)

SELECT u.username
FROM usuario as u
WHERE u.username NOT IN (
	SELECT DISTINCT u.username
FROM usuario as u
INNER JOIN usuario_rol as ur ON u.id_usuario = ur.id_usuario
inner join roles r on r.id_rol=ur.id_rol
inner join rol_permisos rp on rp.id_rol=r.id_rol
inner join permisos p on p.id_permiso=rp.id_permiso
inner join modulos m on m.id_modulo=p.id_modulo
where m.modulo='ADMIN'
)

--select por modulo
select 
u.username,
m.modulo
FROM usuario as u
INNER JOIN usuario_rol as ur ON u.id_usuario = ur.id_usuario
inner join roles r on r.id_rol=ur.id_rol
inner join rol_permisos rp on rp.id_rol=r.id_rol
inner join permisos p on p.id_permiso=rp.id_permiso
inner join modulos m on m.id_modulo=p.id_modulo
where m.modulo='ADMON'

