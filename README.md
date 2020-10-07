# prmToolkit

# ArgumentsValidator
Classe responsável por gerenciar validações de argumentos.

Podemos realizar validações indivíduais ou em grupos.

É possível levantar uma exceção ou captura-las.

### Installation - ArgumentsValidator

Para instalar, abra o prompt de comando Package Manager Console do seu Visual Studio e digite o comando abaixo:

Para adicionar somente a referencia a dll
```sh
Install-Package prmToolkit.ArgumentsValidator
```

Para adicionar somente as classes
```sh
Install-Package prmToolkit.ArgumentsValidator-Source
```
### Exemplo de como usar

```sh
namespace prmToolkit.Test
{
    [TestClass]
    public class ValidationTest
    {
        /// <summary>
        /// Este método captura as mensagens das exceções lançadas pelos argumentos 
        /// mas não é lançada uma exceção para o usuário
        /// </summary>
        [TestMethod]
        public void ObterListaDeMensagensDasExcecoes()
        {
            List<string> result = ArgumentsValidator.GetMessagesFromExceptions(
                                    RaiseException.IfNull(null, "object is required"),
                                    RaiseException.IfNotEmail("email_invalid", "email invalid")
                );

            Assert.IsNotNull(result, "object required");
            Assert.IsTrue(result.Count == 2, "There should be two exceptions");
        }
        /// <summary>
        /// Este método captura as exceções lançadas pelos os argumentos 
        /// mas não é lançada uma exceção para o usuário
        /// </summary>
        [TestMethod]
        public void ObterListaDeExecoesSemLancar()
        {
            List<Exception> result = ArgumentsValidator.GetExceptionList(
                                    RaiseException.IfNull(null, "object is required"),
                                    RaiseException.IfNotEmail("email_invalid", "email invalid")
                );

            Assert.IsNotNull(result, "object required");
            Assert.IsTrue(result.Count == 2, "There should be two exceptions");
        }

        /// <summary>
        /// Este método lança uma única exceção, com as mensagens das exceções geradas pelos os argumentos 
        /// </summary>
        [TestMethod]
        public void LancarUnicaExcecaoComMensagensDoGrupoDeExcecoes()
        {
            try
            {
                ArgumentsValidator.RaiseExceptionOfInvalidArguments(
                                            RaiseException.IfNull(null, "object is required"),
                                            RaiseException.IfNotEmail("email_invalid", "email invalid")
                                            );

                
            }
            catch (Exception ex)
            {

                Assert.IsTrue(ex.Message.Contains("object is required") && ex.Message.Contains("email invalid"), "There should be two exceptions");
            }
        }

        /// <summary>
        /// Este método lança uma única exceção, com uma única mensagem que represanta as exceções geradas pelos os argumentos 
        /// </summary>
        [TestMethod]
        public void LancarUnicaExcecaoComUnicaMensagemDoGrupoDeExcecoes()
        {
            try
            {
                bool existe = true;

                ArgumentsValidator.RaiseExceptionOfInvalidArguments("Dados inválidos",
                                            RaiseException.IfTrue(existe),
                                            RaiseException.IfNotEmail("paulo.com.br")
                                            );

            }
            catch (Exception ex)
            {

                Assert.IsTrue(ex.Message.Contains("Dados inválidos"), "There should be two exceptions");
            }
        }

        /// <summary>
        /// Este método lança uma exceção indivídual para o usuário
        /// </summary>
        [TestMethod]
        public void LancarExcecaoIndividual()
        {
            try
            {
                RaiseException.IfNotNull(null, "object is required", true);
            }
            catch (Exception ex)
            {
                Assert.AreEqual(ex.Message, "object is required", "is expected value not null");
            }
        }
    }
}

```

# VEJA TAMBÉM
## Grupo de Estudo no Telegram
- [Participe gratuitamente do grupo de estudo](https://t.me/blogilovecode)

## Cursos baratos!
- [Meus cursos](https://olha.la/udemy)

## Novidades, cupons de descontos e cursos gratuitos
https://olha.la/ilovecode-receber-cupons-novidades
