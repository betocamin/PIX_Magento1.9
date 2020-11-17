# PIX_Magento1.9
Meio de Pagamento PIX com QR Code estático para Magento 1.9

Adiciona um novo método de pagamento para ser utilizado com o PIX com Qr Code, sem valor do pedido e sem código identificador.

----------------------------
INSTALAÇÃO
----------------------------

1 - Extrair os arquivos na pasta raiz;
2 - Para enviar o QR Code no email do pedido, você deve editar o arquivo /app/locale/pt_BR/template/email/sales/order_new.phtml.
Procure pelo código {{var payment_html}} e coloque abaixo dele {{block type='core/template' area='frontend' template='pixnoemail/pix.phtml' order=$order}} , desta forma o QR Code será exibido no email enviado para o cliente caso o pagamento seja via PIX.
3 - Fazer um cópia do arquivo app/design/frontend/base/default/template/checkout/success.phtml para app/design/frontend/seu_pacote/seu_tema/template/checkout/success.phtml e inserir o código abaixo entre as linhas 39 e 40:

<?php if ($order->getPayment()->getMethod() == "pixbasico"){ ?>
<?php
$order = Mage::getModel ( 'sales/order' );
$order->loadByIncrementId ( $this->getOrderId () );
$totals  = $order->getTotals();
$method = $order->getPayment ()->getMethod ();
if (strpos ( $method, 'pixbasico' ) !== false) {
	echo $this->__ ( '<div style=" padding: 20px; background-color: #f5f5f5; margin: 10px 0; border: grey 1px dashed;"><p style="color: #ee001c"><strong>EFETUE O PAGAMENTO PELO QRCODE OU PELO IDENTIFICADOR:</strong></p><br>'.'<strong>Valor: R$ '.number_format($order->getGrandTotal(), 2, ',', '.').'</strong><br><br>Identificador 00.000.000/0000-00<br><br><img src="https://www.seusite.com/media/wysiwyg/pix/qrcode.gif" alt="QR Code IDENTIFICADOR 00.000.000/0000-00" /><br><br>Envie o comprovante do PIX para o email <a href="mailto:email@seuemail.com?Subject=Comprovante - PIX" target="_top">email@seuemail.com</a>.</div>');
}
?>
<?php } ?>
