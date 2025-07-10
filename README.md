/**
 * @NApiVersion 2.x
 * @NScriptType Suitelet
 */
define(['N/ui/serverWidget', 'N/record', 'N/file', 'N/redirect', 'N/runtime'], function(ui, record, file, redirect, runtime) {
    function onRequest(context) {
        if (context.request.method === 'GET') {
            var form = ui.createForm({ title: 'Fale com o SAC Dylan e Duonn' });

            // CSS incorporado
  form.addField({
    id: 'custpage_css',
    type: ui.FieldType.INLINEHTML,
    label: ' '
}).defaultValue = '<style>' +
'.uir-main-content, .uir-outside-fields, .uir-inline-tag, .uir-field-wrapper, .uir-field, .uir-label, .uir-field-input {' +
    'width: 100% !important;' +
    'max-width: 700px !important;' +
    'margin: 0 auto !important;' +
    'box-sizing: border-box;' +
    'float: none !important;' +
    'display: block !important;' +
'}' +
'body {' +
    'font-family: Arial, sans-serif;' +
    'background-color: #ffffff;' +
    'padding: 20px;' +
'}' +
'input, select, textarea {' +
    'width: 100% !important;' +
    'padding: 10px;' +
    'margin-top: 6px;' +
    'border-radius: 5px;' +
    'border: 1px solid #ccc;' +
    'box-sizing: border-box;' +
    'font-size: 14px;' +
'}' +
'.uir-button {' +
    'margin-top: 20px !important;' +
    'background-color: #000 !important;' +
    'color: white !important;' +
    'padding: 12px !important;' +
    'width: 100% !important;' +
    'border: none !important;' +
    'border-radius: 6px !important;' +
    'cursor: pointer !important;' +
    'font-size: 16px !important;' +
    'font-weight: 600 !important;' +
'}' +
'.uir-button:hover {' +
    'background-color: #222 !important;' +
'}' +
'@media (max-width: 768px) {' +
    'input, select, textarea {' +
        'font-size: 15px !important;' +
    '}' +
'}' +
'.uir-field-wrapper {' +
    'float: none !important;' +
    'clear: both !important;' +
'}' +
'form[name="main_form"] table[border="0"] tr {' +
    'display: block;' +
    'width: 100%;' +
'}' +
'</style>';


form.addField({
    id: 'custpage_logo',
    type: ui.FieldType.INLINEHTML,
    label: ' '
}).defaultValue = '<div style="text-align:center; margin-bottom: 30px;">' +
    '<img src="https://9733457-sb1.app.netsuite.com/core/media/media.nl?id=20364&c=9733457_SB1&h=hCw5cbRuap85ocxPalYycNyWouBG4m74DU6oZn0miaAmRKeG" style="max-width:150px;" />' +
    '<h1 style="font-size:1.8rem; font-weight:bold; color:#000;">Fale com o SAC <br>Dylan e Duonn</h1>' +
'</div>';



            // Campos do formulário
            form.addField({ id: 'custpage_nome', type: ui.FieldType.TEXT, label: 'Nome*' }).isMandatory = true;
            form.addField({ id: 'custpage_sobrenome', type: ui.FieldType.TEXT, label: 'Sobrenome*' }).isMandatory = true;
            form.addField({ id: 'custpage_cpf', type: ui.FieldType.TEXT, label: 'CPF/CNPJ*' }).isMandatory = true;
            form.addField({ id: 'custpage_email', type: ui.FieldType.EMAIL, label: 'E-mail*' }).isMandatory = true;

            var subField = form.addField({ id: 'custpage_subsidiaria', type: ui.FieldType.SELECT, label: 'Subsidiária*' });
            subField.addSelectOption({ value: '', text: '' });
            subField.addSelectOption({ value: 'dylan', text: 'Dylan do Brasil' });
            subField.addSelectOption({ value: 'dunn', text: 'Duonn Sound' });
            subField.isMandatory = true;

            form.addField({ id: 'custpage_titulo', type: ui.FieldType.TEXT, label: 'Título*' }).isMandatory = true;

            var prodField = form.addField({ id: 'custpage_produto', type: ui.FieldType.SELECT, label: 'Produto*' });
            prodField.addSelectOption({ value: '', text: '' });
            var produtos = [ "UDX-01 MULTI", "UDX-02 MULTI", "UDX-03 MULTI", "UDX-05", "UDX-06", "UDX-33",
                "DW-602 MAX", "DW-902 POWER", "D-8000 S", "D-8500 S", "D-9000 S", "D-9001 S", "D-9003 S", "D-9005 S",
                "D-9006 S", "D-9500", "D-9501", "DM/DS", "DM-5", "DM-7", "DM-8", "DS-81", "QS", "QS-5", "QS-10", "QS-10/M2",
                "DSM", "DSM-300", "DSM-301", "DSM 600 - SIMPLES", "DSM 600 - SUPER STEREO", "DSM 601  - SIMPLES",
                "DSM 601 - SUPER STEREO", "DSM-900", "DSM-901", "SMD", "SMD-100", "SMD-57", "SMD-58 PLUS", "DLS",
                "DLS-8", "DLS-9", "DLS-10+", "DLS-20", "DD", "DD-1", "DD-2", "DD-7", "DD-9", "DA", "DA-400", "DA-800",
                "DH", "DH-55", "DH-77", "DH-88", "DG", "DG-10", "DG-50", "DG-86 BLACK", "DG-86 WHITE", "DG-90", "DE",
                "DE-115", "DE-215", "DE-225 BLACK", "DE-225 WHITE", "DE- 635", "DE-845", "DL", "DL-400", "DL-640",
                "DL-700", "DL-840", "GX", "GX-1", "ATRIUM 12", "ATRIUM 20", "AXIOS 16", "AXIOS 24", "VIRTUS 480", "PRISMA 480"
            ];
            produtos.forEach(function(prod) {
                prodField.addSelectOption({ value: prod, text: prod });
            });

            var tipoField = form.addField({ id: 'custpage_tipo', type: ui.FieldType.SELECT, label: 'Tipo de Suporte*' });
            tipoField.addSelectOption({ value: '', text: '' });
            tipoField.addSelectOption({ value: 'assistencia', text: 'Assistência Técnica' });
            tipoField.addSelectOption({ value: 'garantia', text: 'Garantia (30 dias)' });
            tipoField.addSelectOption({ value: 'duvida', text: 'Dúvida' });
            tipoField.addSelectOption({ value: 'outro', text: 'Outro' });
            tipoField.isMandatory = true;

            form.addField({ id: 'custpage_file', type: ui.FieldType.FILE, label: 'Anexar imagem/vídeo' });
            form.addField({ id: 'custpage_descricao', type: ui.FieldType.TEXTAREA, label: 'Descreva o problema*' }).isMandatory = true;

            form.addSubmitButton({ label: 'Enviar' });

            context.response.writePage(form);
        } else if (context.request.method === 'POST') {
            var customRec = record.create({ type: 'customrecord2233', isDynamic: true });

            customRec.setValue({ fieldId: 'custrecord_sac_nome', value: context.request.parameters.custpage_nome });
            customRec.setValue({ fieldId: 'custrecord_sac_sobrenome', value: context.request.parameters.custpage_sobrenome });
            customRec.setValue({ fieldId: 'custrecord_sac_cpf', value: context.request.parameters.custpage_cpf });
            customRec.setValue({ fieldId: 'custrecord_sac_email', value: context.request.parameters.custpage_email });
            customRec.setValue({ fieldId: 'custrecord_sac_subsidiaria', value: context.request.parameters.custpage_subsidiaria });
            customRec.setValue({ fieldId: 'custrecord_sac_titulo', value: context.request.parameters.custpage_titulo });
            customRec.setValue({ fieldId: 'custrecord_sac_produto', value: context.request.parameters.custpage_produto });
            customRec.setValue({ fieldId: 'custrecord_sac_tipo', value: context.request.parameters.custpage_tipo });
            customRec.setValue({ fieldId: 'custrecord_sac_descricao', value: context.request.parameters.custpage_descricao });

            if (context.request.files && context.request.files.custpage_file) {
                var uploadedFile = context.request.files.custpage_file;
                uploadedFile.folder = 8181; // ajuste conforme necessário
                var fileId = uploadedFile.save();
                customRec.setValue({ fieldId: 'custrecord_sac_file', value: fileId });
            }

            customRec.save();

            var form = ui.createForm({ title: 'Fale com o SAC Dylan e Duonn' });
            form.addField({
                id: 'custpage_msg',
                type: ui.FieldType.INLINEHTML,
                label: ' '
            }).defaultValue = '<h2 style="color:green;text-align:center;">Formulário enviado com sucesso! Em breve entraremos em contato.</h2>';
            context.response.writePage(form);
        }
    }
    return { onRequest: onRequest };
});
