# Criando-um-Jogo-da-Forca-com-uma-Aplica-o-Console-Java
Projeto: Jogo da Forca em Java
Descrição: Jogo de console orientado a objetos, com uso de classes como Palavra, Forca, JogoDaForca e Util.

Arquivos incluídos:
- Palavra.java
- Forca.java
- Util.java
- JogoDaForca.java

Funcionalidades:
- Palavra oculta inserida manualmente
- Interface interativa via terminal
- Controle de tentativas
- ASCII art da forca
- Verificação de letras usadas
- Mensagens de vitória ou derrota
import java.util.*;

// Jogo da Forca em Java com POO – versão compacta em um único arquivo
public class JogoDaForcaCompleto {

    public static void main(String[] args) {
        Util.limparTela();
        System.out.println("Bem-vindo ao Jogo da Forca!");

        String palavra = Util.lerEntrada("Informe a palavra secreta: ");
        Util.limparTela();

        Palavra palavraObj = new Palavra(palavra);
        Set<Character> letrasUsadas = new HashSet<>();
        int tentativas = 6;

        while (tentativas > 0) {
            System.out.println("\nPalavra: " + palavraObj.mostrarEstadoAtual());
            System.out.println("Tentativas restantes: " + tentativas);
            Forca.desenhar(6 - tentativas);
            System.out.println("Letras usadas: " + letrasUsadas);

            String entrada = Util.lerEntrada("Digite uma letra: ");
            if (entrada.length() != 1 || !Character.isLetter(entrada.charAt(0))) {
                System.out.println("Entrada inválida. Digite apenas uma letra.");
                continue;
            }

            char letra = entrada.charAt(0);
            if (letrasUsadas.contains(letra)) {
                System.out.println("Você já tentou essa letra!");
                continue;
            }

            letrasUsadas.add(letra);

            if (palavraObj.contemLetra(letra)) {
                palavraObj.adicionarLetraCorreta(letra);
                System.out.println("Letra correta!");
            } else {
                tentativas--;
                System.out.println("Letra incorreta!");
            }

            if (palavraObj.foiReveladaPorCompleto()) {
                Util.limparTela();
                System.out.println("Você venceu! A palavra era: " + palavra);
                return;
            }
        }

        Util.limparTela();
        Forca.desenhar(6);
        System.out.println("💀 Você perdeu! A palavra era: " + palavra);
    }

    // Classe Palavra
    static class Palavra {
        private final String palavraSecreta;
        private final Set<Character> letrasCorretas = new HashSet<>();

        public Palavra(String palavra) {
            this.palavraSecreta = palavra.toUpperCase();
        }

        public boolean contemLetra(char letra) {
            return palavraSecreta.indexOf(letra) >= 0;
        }

        public void adicionarLetraCorreta(char letra) {
            letrasCorretas.add(letra);
        }

        public boolean foiReveladaPorCompleto() {
            for (char c : palavraSecreta.toCharArray()) {
                if (!letrasCorretas.contains(c)) return false;
            }
            return true;
        }

        public String mostrarEstadoAtual() {
            StringBuilder estado = new StringBuilder();
            for (char c : palavraSecreta.toCharArray()) {
                estado.append(letrasCorretas.contains(c) ? c : '_').append(' ');
            }
            return estado.toString().trim();
        }
    }

    // Classe Forca (ASCII Art)
    static class Forca {
        public static void desenhar(int erros) {
            String[] forca = {
                "  +---+",
                "  |   |",
                "      |",
                "      |",
                "      |",
                "      |",
                "========="
            };

            if (erros >= 1) forca[2] = "  O   |";
            if (erros >= 2) forca[3] = "  |   |";
            if (erros >= 3) forca[3] = " /|   |";
            if (erros >= 4) forca[3] = " /|\\  |";
            if (erros >= 5) forca[4] = " /    |";
            if (erros >= 6) forca[4] = " / \\  |";

            for (String linha : forca) System.out.println(linha);
        }
    }

    // Classe Util (entrada e limpeza)
    static class Util {
        private static final Scanner scanner = new Scanner(System.in);

        public static String lerEntrada(String mensagem) {
            System.out.print(mensagem);
            return scanner.nextLine().trim().toUpperCase();
        }

        public static void limparTela() {
            System.out.print("\033[H\033[2J");
            System.out.flush();
        }
    }
}
