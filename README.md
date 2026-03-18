import java.time.LocalDateTime;
import java.util.*;
import java.io.*;

public class Main {

    static class Evento {
        String nome;
        String endereco;
        String categoria;
        LocalDateTime horario;
        String descricao;

        Evento(String nome, String endereco, String categoria, LocalDateTime horario, String descricao) {
            this.nome = nome;
            this.endereco = endereco;
            this.categoria = categoria;
            this.horario = horario;
            this.descricao = descricao;
        }

        public String toFile() {
            return nome + ";" + endereco + ";" + categoria + ";" + horario + ";" + descricao;
        }

        public static Evento fromFile(String linha) {
            String[] d = linha.split(";");
            return new Evento(d[0], d[1], d[2], LocalDateTime.parse(d[3]), d[4]);
        }

        public String toString() {
            return "\nEvento: " + nome +
                   "\nEndereço: " + endereco +
                   "\nCategoria: " + categoria +
                   "\nData: " + horario +
                   "\nDescrição: " + descricao +
                   "\n----------------------";
        }
    }

    static List<Evento> eventos = new ArrayList<>();
    static List<Evento> confirmados = new ArrayList<>();

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        carregar();

        while (true) {
            System.out.println("\n1 - Cadastrar evento");
            System.out.println("2 - Listar eventos");
            System.out.println("3 - Participar");
            System.out.println("4 - Ver confirmados");
            System.out.println("5 - Cancelar participação");
            System.out.println("0 - Sair");

            int op = sc.nextInt();
            sc.nextLine();

            switch (op) {
                case 1:
                    System.out.println("Nome:");
                    String nome = sc.nextLine();

                    System.out.println("Endereço:");
                    String endereco = sc.nextLine();

                    System.out.println("Categoria:");
                    String categoria = sc.nextLine();

                    System.out.println("Data (YYYY-MM-DDTHH:MM):");
                    LocalDateTime data = LocalDateTime.parse(sc.nextLine());

                    System.out.println("Descrição:");
                    String desc = sc.nextLine();

                    eventos.add(new Evento(nome, endereco, categoria, data, desc));
                    salvar();
                    break;

                case 2:
                    eventos.sort(Comparator.comparing(e -> e.horario));
                    for (int i = 0; i < eventos.size(); i++) {
                        Evento e = eventos.get(i);
                        System.out.println("\nÍndice: " + i);
                        System.out.println(e);

                        if (e.horario.isBefore(LocalDateTime.now())) {
                            System.out.println("Status: Já ocorreu");
                        } else {
                            System.out.println("Status: Próximo");
                        }
                    }
                    break;

                case 3:
                    System.out.println("Digite o índice:");
                    int i = sc.nextInt();
                    confirmados.add(eventos.get(i));
                    break;

                case 4:
                    confirmados.forEach(System.out::println);
                    break;

                case 5:
                    System.out.println("Digite o índice:");
                    int j = sc.nextInt();
                    confirmados.remove(j);
                    break;

                case 0:
                    System.exit(0);
            }
        }
    }

    static void salvar() {
        try (BufferedWriter w = new BufferedWriter(new FileWriter("events.dat"))) {
            for (Evento e : eventos) {
                w.write(e.toFile());
                w.newLine();
            }
        } catch (Exception e) {
            System.out.println("Erro ao salvar");
        }
    }

    static void carregar() {
        File f = new File("events.dat");
        if (!f.exists()) return;

        try (BufferedReader r = new BufferedReader(new FileReader(f))) {
            String linha;
            while ((linha = r.readLine()) != null) {
                eventos.add(Evento.fromFile(linha));
            }
        } catch (Exception e) {
            System.out.println("Erro ao carregar");
        }
    }
}# java
pratique imersão digital 
