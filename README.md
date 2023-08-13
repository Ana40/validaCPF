    //# validaCPF
     //Classe em Apex para utilizar  em uma ação do flow para fazer busca e validar o cpf encontrado.
public class ValidaCPF {

    // O que é @InvocableMethod? O que faz? Para que serve cada um dos parâmetros?
    // O método é uma anotação que permite ser chamado a apartir da interface do usuário ou pelo código Apex.
    // A anotação possui dois parametros:"rótulo"(é uma string usada como o nome de exibição do método na interface do usuário) 
    // e "descrição"(é uma string que fornece uma breve explicação sobre o que o método faz).

    @InvocableMethod(label='Validador de CPF' description='Função para calcular se o CPF é Válido')
    public static List<Boolean> validarCPF(List<String> cpfList) {
   		
    // O que a função trim() faz? Para qual cenário ela serve?
    // O trim() serve para remover quaisquer espaços em branco iniciais ou finais da string "CPF"
    // O cenário que pode serusado por exemplo em uma entrada do usuário ou dados que foram lidos de um arquivo, 
    //pois pode ajudar a garantir que os dados estejam no formato esperado.
        
    // O que a função replaceAll faz? Quantos parametros ela possui? Para que serve cada um dos parâmetros? 
    // Ela substitui todas as ocorrências de uma expressão regular ou string especificada por uma nova string.
    // Ela possui 2 parâmentros.
    // O 1° parâmetro é a expressão regular ou string a ser substituída.
    // O 2° parâmetro é a string de substituição.

    String cpf = cpfList.get(0);
           
    cpf = cpf.trim().replaceAll('[^0-9]', '');
        
    // Para que serve essa verificação?
    // É usada para limpar e formatar uma string que representa um número de identificação fiscal
    // brasileiro como o CPF.
    // Então a trim() remove os espaços em branco e o replaceAll('[^0-9]', '') substiui todos os caracteres que não
    // são dígitos(0-9) por uma string vazia removendo assim os caracteres não numéricos da string.

    if (
	(cpf.length() != 11) ||
    cpf.equals('00000000000') || cpf.equals('11111111111') ||
    cpf.equals('22222222222') || cpf.equals('33333333333') ||
    cpf.equals('44444444444') || cpf.equals('55555555555') ||
    cpf.equals('66666666666') || cpf.equals('77777777777') ||
    cpf.equals('88888888888') || cpf.equals('99999999999')
	)
    return new List<Boolean>{false};

     // Calculo para o primeiro digito verificador        
     Integer soma = 0;
     Integer peso = 10;
        
     List<String> cpfSplit = cpf.substring(0,9).split('');
        
     for(String digito : cpfSplit) {
     soma = soma + (Integer.valueOf(digito) * peso);peso = peso - 1;
     }
        
     Integer resto = 11 - (math.mod(soma, 11));
     Integer primeiroDigitoVerificador;
     if(resto > 10) {primeiroDigitoVerificador = 0;
     } else {primeiroDigitoVerificador = resto;
     }
        
     // Calculo para o segundo digito verificador
     soma = 0;
     peso = 11;
        
     List<String> cpfSplit2 = cpf.substring(0,10).split('');
	
     for(String digito : cpfSplit2) {soma = soma + (Integer.valueOf(digito) * peso);peso = peso - 1;
     }
        
     resto = 11 - (math.mod(soma,11));
        
     Integer segundoDigitoVerificador;
     if(resto > 10) {segundoDigitoVerificador = 0;
     } else {segundoDigitoVerificador = resto;
     }
        
     // Verificação dos dois últimos digitos
     if(primeiroDigitoVerificador != Integer.valueOf(cpf.substring(9,10)) || segundoDigitoVerificador != Integer.ValueOf(cpf.substring(10,11))){return new List<Boolean>{false};
     }
        
     return new List<Boolean>{true};
    }
}
