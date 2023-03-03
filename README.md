# WEBSITE - PLATAFORMA ADMINISTRATIVA DE ACÇÃO SOCIAL - CAMARA MUNICIPAL DE OLHÃO (IMCOMPLETO)

Link do Projecto: https://meocloud.pt/link/d8c47845-1016-4d41-bec3-275c3c53e2bc/Accao_Social.zip/

username: admin
password: admin

Plataforma: http://app.cm-olhao.pt/asocial/

Caso que queira modificar a base de dados esta localizado na pasta modules\core\src\pt\cmolhao onde esta o ficheiro app.properties

### Database 
cuba.dataSource.username = postgres
cuba.dataSource.password = readln
cuba.dataSource.dbName = postgres
cuba.dataSource.host = localhost
cuba.dataSource.port =
cuba.dataSource.connectionParams = ?currentSchema=sc

### Exemplo CRUD (Casos Sociais)

Pasta (Localizacao): modules\web\src\pt\cmolhao\web\casos_sociais

#### Index (Search)

##### XML (Design)

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<window xmlns="http://schemas.haulmont.com/cuba/screen/window.xsd"
        xmlns:c="http://schemas.haulmont.com/cuba/screen/jpql_condition.xsd"
        caption="msg://browseCaption"
        focusComponent="documentacaoCasosSociaisesTable"
        messagesPack="pt.cmolhao.web.casos_sociais">
    <data readOnly="true">
        <collection id="documentacaoCasosSociaisesDc"
                    class="pt.cmolhao.entity.DocumentacaoCasosSociais"
                    view="documentacaoCasosSociais-view">
            <loader id="documentacaoCasosSociaisesDl">
                <query>
                    <![CDATA[select e from cmolhao_DocumentacaoCasosSociais e]]>
                    <condition>
                        <and>
                            <c:jpql>
                                <c:where>e.numeroDocumentacao like :numeroDocumentacao</c:where>
                            </c:jpql>
                        </and>
                    </condition>
                </query>
            </loader>
        </collection>
    </data>
    <dialogMode height="600"
                width="800"/>
    <layout expand="documentacaoCasosSociaisesTable"
            spacing="true">
        <groupBox caption="Filtros de Pesquisa"
                  collapsable="true"
                  spacing="true"
                  icon="font-icon:FILTER"
                  width="100%">

            <scrollBox spacing="true">


                <grid spacing="true" width="100%">
                    <columns>
                        <column/>
                        <column flex="1"/>
                    </columns>
                    <rows>
                        <row>

                            <textField id="numeroDocumentacaoField"
                                       property="numeroDocumentacao"
                                       width="250px"
                                       icon="font-icon:FILE"
                                       inputPrompt="(Introduza o numero documento)"
                                       caption="Número do Documento: "/>
                        </row>
                    </rows>
                </grid>

                <hbox spacing="true">
                    <button id="reset_documentacao_casos_sociais" icon="ERASER" caption="Limpar" />
                    <button id="search_documentacao_casos_sociais" caption="Pesquisar" icon="SEARCH" stylename="info" />
                </hbox>

            </scrollBox>
        </groupBox>

        <hbox spacing="true">
            <lookupField id="linhasDocumentacao"
                         caption="Mostrar Numero de Linhas"
                         inputPrompt="(seleccione o numero de linhas)"
                         icon="ARROWS_V"/>
        </hbox>
        <groupTable id="documentacaoCasosSociaisesTable"
                    width="100%"
                    dataContainer="documentacaoCasosSociaisesDc"
                    emptyStateMessage="Não Existe Dados na Tabela"
                    emptyStateLinkMessage="Adicione Dados na Tabela(Ctrl+\)">
            <actions>
                <action id="create" type="create"/>
                <action id="edit" type="edit"/>
                <action id="remove" type="remove"/>
                <action id="excel" type="excel"/>

            </actions>
            <columns>
                <column id="idRedeTrabalho" caption="Rede de Trabalho"/>
                <column id="idSubRedeTrabalho" caption="Sub Rede de Trabalho"/>
                <column id="numeroDocumentacao" caption="Número do Documentação"/>
                <column id="dataDocumento" caption="Data do Ficheiro"/>

                <column id="nomeFicheiro" caption="Nome do Ficheiro"/>

                <column id="file" caption="Ficheiro"/>
            </columns>
            <rowsCount/>
            <buttonsPanel id="buttonsPanel"
                          alwaysVisible="true">
                <button id="createBtn" action="documentacaoCasosSociaisesTable.create" caption="Criar" icon="font-icon:PLUS" stylename="primary"/>
                <button id="editBtn" action="documentacaoCasosSociaisesTable.edit" caption="Editar" icon="font-icon:PENCIL" stylename="info"/>
                <button id="removeBtn" action="documentacaoCasosSociaisesTable.remove" caption="Remover" icon="font-icon:TRASH" stylename="danger"/>
                <button id="excelBtn" action="documentacaoCasosSociaisesTable.excel" stylename="friendly" />
            </buttonsPanel>
        </groupTable>
        <hbox id="lookupActions" spacing="true" visible="false">
            <button action="lookupSelectAction"/>
            <button action="lookupCancelAction"/>
        </hbox>
    </layout>
</window>
```

##### Java (Funcionamento)


```java
@UiController("cmolhao_DocumentacaoCasosSociais.browse")
@UiDescriptor("documentacao-casos-sociais-browse.xml")
@LookupComponent("documentacaoCasosSociaisesTable")
@LoadDataBeforeShow
public class DocumentacaoCasosSociaisBrowse extends StandardLookup<DocumentacaoCasosSociais> {
    @Inject
    protected CollectionLoader<DocumentacaoCasosSociais> documentacaoCasosSociaisesDl;
    @Inject
    protected LookupField linhasDocumentacao;
    @Inject
    protected TextField<String> numeroDocumentacaoField;
    @Inject
    protected GroupTable<DocumentacaoCasosSociais> documentacaoCasosSociaisesTable;
    @Named("documentacaoCasosSociaisesTable.remove")
    protected RemoveAction<DocumentacaoCasosSociais> documentacaoCasosSociaisesTableRemove;

    @Inject
    protected UiComponents uiComponents;


    @Inject
    private ExportDisplay exportDisplay;

    @Inject
    protected ScreenBuilders screenBuilders;

    @Inject
    private Notifications notifications;

    @Inject
    private Dialogs dialogs;

    public static boolean isNumeric(String str) {
        return str != null && str.matches("[-+]?\\d*\\.?\\d+");
    }


    @Subscribe
    protected void onInit(InitEvent event) {

        List<Integer> list = new ArrayList<>();
        list.add(10);
        list.add(20);
        list.add(50);
        list.add(100);
        list.add(200);
        list.add(500);
        list.add(1000);
        list.add(2000);
        list.add(5000);
        list.add(10000);
        linhasDocumentacao.setOptionsList(list);

        documentacaoCasosSociaisesTable.setEmptyStateLinkClickHandler(emptyStateClickEvent ->
                screenBuilders.editor(documentacaoCasosSociaisesTable)
                        .newEntity()
                        .withInitializer(customer -> {
                            if (numeroDocumentacaoField.getValue() != null) {
                                if (isNumeric(numeroDocumentacaoField.getValue())) {
                                    customer.setNumeroDocumentacao(numeroDocumentacaoField.getValue());
                                } else {
                                    notifications.create()
                                            .withCaption("<code>Erro ao atribuir string do número de documentação </code>")
                                            .withDescription("<u>Devera introduzir um numero inteiro</u>")
                                            .withType(Notifications.NotificationType.ERROR)
                                            .withContentMode(ContentMode.HTML)
                                            .show();
                                }
                            }

                        })
                        .withScreenClass(DocumentacaoCasosSociaisEdit.class)    // specific editor screen
                        .build()
                        .show()
        );


        documentacaoCasosSociaisesTable.addGeneratedColumn("file", entity -> {
            if(entity.getFile() != null) {
                Button btn = uiComponents.create(Button.class);
                btn.setCaption(entity.getFile().getName());
                btn.addStyleName("download_documentos");
                String ext = entity.getFile().getExtension();
                btn.setIcon("font-icon:FILE_O");

                if (ext.equals("pdf")) {
                    btn.setIcon("font-icon:FILE_PDF_O");
                }
                if (ext.equals("docx") || ext.equals("doc")) {
                    btn.setIcon("font-icon:FILE_WORD_O");
                }
                if (ext.equals("webm") || ext.equals("mp4") || ext.equals("mpg") || ext.equals("ogg") || ext.equals("avi") || ext.equals("mov")) {
                    btn.setIcon("font-icon:FILE_VIDEO_O");
                }
                if (ext.equals("wav") || ext.equals("mp3")) {
                    btn.setIcon("font-icon:FILE_SOUND_O");
                }
                if (ext.equals("jpg") || ext.equals("jpeg") || ext.equals("png") || ext.equals("gif")) {
                    btn.setIcon("font-icon:FILE_PICTURE_O");
                }
                if (ext.equals("rar") || ext.equals("zip")) {
                    btn.setIcon("font-icon:FILE_ZIP_O");
                }
                if (ext.equals("xlsx") || ext.equals("xls") || ext.equals("csv")) {
                    btn.setIcon("font-icon:FILE_EXCEL_O");
                }
                if (ext.equals("pptx") || ext.equals("ppt")) {
                    btn.setIcon("font-icon:FILE_POWERPOINT_O");
                }
                btn.setAction(new BaseAction("download") {
                    @Override
                    public void actionPerform(Component component) {
                        FileDescriptor imageFile = entity.getFile();
                        exportDisplay.show(imageFile, ExportFormat.OCTET_STREAM);

                    }
                });
                return btn;
            }
            else
            {
                return null;
            }
        });

    }



    @Subscribe
    protected void onAfterShow(AfterShowEvent event) {
        getWindow().setCaption("Listar Documentação dos Casos Sociais");
    }

    @Subscribe("reset_documentacao_casos_sociais")
    protected void onReset_documentacao_casos_sociaisClick(Button.ClickEvent event) {
        numeroDocumentacaoField.setValue(null);
        documentacaoCasosSociaisesDl.removeParameter("numeroDocumentacao");
        documentacaoCasosSociaisesDl.load();
    }

    @Subscribe("search_documentacao_casos_sociais")
    protected void onSearch_documentacao_casos_sociaisClick(Button.ClickEvent event) {
        if (numeroDocumentacaoField.getValue() != null) {
            if (isNumeric(numeroDocumentacaoField.getValue()))
            {
                documentacaoCasosSociaisesDl.setParameter("numeroDocumentacao", "(?i)%" + numeroDocumentacaoField.getValue() + "%" );
            }
            else
            {
                notifications.create()
                        .withCaption("<code>Erro ao atribuir string do número de documentação</code>")
                        .withDescription("<u>Devera introduzir um numero inteiro</u>")
                        .withType(Notifications.NotificationType.ERROR)
                        .withContentMode(ContentMode.HTML)
                        .show();
            }

        } else {
            documentacaoCasosSociaisesDl.removeParameter("numeroDocumentacao");
        }


        documentacaoCasosSociaisesDl.load();
    }

    @Subscribe("documentacaoCasosSociaisesTable.remove")
    protected void onDocumentacaoCasosSociaisesTableRemove(Action.ActionPerformedEvent event) {
        documentacaoCasosSociaisesTableRemove.setConfirmation(false);
        if (documentacaoCasosSociaisesTable.getSelected().isEmpty())
        {
            dialogs.createOptionDialog()
                    .withCaption("Selecção de documentos dos casos sociais")
                    .withMessage("Deve seleccionar pelo um dos documentos dos casos sociais")
                    .withActions(
                            new DialogAction(DialogAction.Type.CLOSE)
                    )
                    .show();
        }
        else
        {
            DocumentacaoCasosSociais user = documentacaoCasosSociaisesTable.getSingleSelected();
            dialogs.createOptionDialog()
                    .withCaption("Remover a linha da tabela documento dos casos sociais número '"+user.getId()+"' ")
                    .withMessage("Tens a certeza que quer remover esta linha da tabela documento dos casos sociais número '"+user.getId()+"'?")
                    .withActions(
                            new DialogAction(DialogAction.Type.YES)
                                    .withHandler(e ->
                                    {
                                        documentacaoCasosSociaisesTableRemove.execute();
                                    }),
                            new DialogAction(DialogAction.Type.NO)
                    )
                    .show();
        }
    }

    @Subscribe("linhasDocumentacao")
    protected void onLinhasDocumentacaoValueChange(HasValue.ValueChangeEvent event) {
        if (event.getValue() != null)
        {
            documentacaoCasosSociaisesDl.setMaxResults(Integer.parseInt(event.getValue().toString()));
        }
        else
        {
            documentacaoCasosSociaisesDl.setMaxResults(0);
        }
        documentacaoCasosSociaisesDl.load();
    }
}
```


#### Create / Edit

##### XML (Design)

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<window xmlns="http://schemas.haulmont.com/cuba/screen/window.xsd"
        caption="msg://editorCaption"
        focusComponent="form"
        messagesPack="pt.cmolhao.web.casos_sociais">
    <data>
        <instance id="documentacaoCasosSociaisDc"
                  class="pt.cmolhao.entity.DocumentacaoCasosSociais"
                  view="documentacaoCasosSociais-view">
            <loader/>
        </instance>

        <collection id="redeTrabalhoDc"
                    class="pt.cmolhao.entity.RedeTrabalho"
                    view="redeTrabalho-view">
            <loader id="customersRedeTrabalhoLoader">
                <query>
                    <![CDATA[select e from cmolhao_RedeTrabalho e]]>
                </query>
            </loader>
        </collection>

        <collection id="subRedeTrabalhoDc"
                    class="pt.cmolhao.entity.SubRedeTrabalho"
                    view="subRedeTrabalho-view">
            <loader id="customersSubRedeTrabalhoLoader">
                <query>
                    <![CDATA[select e from cmolhao_SubRedeTrabalho e]]>
                </query>
            </loader>
        </collection>

    </data>
    <dialogMode height="600"
                width="800"/>
    <layout expand="editActions" spacing="true">
        <scrollBox id="scrollBox" spacing="true">

            <tabSheet>
                <tab id="dados_pessoais"
                     caption="Dados Pessoais"
                     icon="font-icon:ID_CARD"
                     margin="true"
                     spacing="true">

                    <form id="form_dados_pessoais">
                        <column width="350px">




                            <lookupPickerField id="idRedeTrabalhoField"
                                               property="idRedeTrabalho"
                                               dataContainer="documentacaoCasosSociaisDc"
                                               required="true"
                                               requiredMessage="Deve indicar a rede de trabalho"
                                               optionsContainer="redeTrabalhoDc"
                                               stylename="id_rede_trabalho"
                                               inputPrompt="(Seleccione a rede trabalho)"
                                               caption="Rede de Trabalho: "
                                               icon="WRENCH">
                                <actions>
                                    <action id="lookup" type="picker_lookup"/>
                                    <action id="clear" type="picker_clear"/>
                                </actions>
                            </lookupPickerField>

                            <textField id="numeroDocumentacaoField" stylename="num_doc" property="numeroDocumentacao" dataContainer="documentacaoCasosSociaisDc" caption="Número do Documento: " icon="font-icon:SORT_NUMERIC_ASC"/>

                            <dateField id="dataDocumentoField" property="dataDocumento" dataContainer="documentacaoCasosSociaisDc" caption="Data do Documento: " visible="false"/>

                            <textField id="nomeFicheiroField" property="nomeFicheiro" dataContainer="documentacaoCasosSociaisDc" caption="Nome do Ficheiro: "  visible="false"/>

                        </column>
                        <column width="350px">

                            <lookupPickerField id="idSubRedeTrabalhoField"
                                               property="idSubRedeTrabalho"
                                               dataContainer="documentacaoCasosSociaisDc"
                                               optionsContainer="subRedeTrabalhoDc"
                                               stylename="id_sub_rede_trabalho"
                                               inputPrompt="(Seleccione a sub-rede trabalho)"
                                               caption="Sub Rede de Trabalho: "
                                               icon="WRENCH">
                                <actions>
                                    <action id="lookup" type="picker_lookup"/>
                                    <action id="clear" type="picker_clear"/>
                                </actions>
                            </lookupPickerField>

                            <textField id="idDocumentacao" property="id" visible="false" dataContainer="documentacaoCasosSociaisDc" />

                        </column>
                    </form>

                </tab>
                <tab id="ficheiro"
                     caption="Ficheiro"
                     icon="font-icon:FILE"
                     margin="true"
                     spacing="true">

                    <form id="form_ficheiro">
                        <column width="450px">
                            <groupBox caption="Ficheiro/Documento" spacing="true" icon="font-icon:FILE" expand="file_documento">
                                <image id="file_documento"
                                       width="150px"
                                       align="MIDDLE_LEFT"
                                       scaleMode="CONTAIN"/>

                                <linkButton id="file_name_text" visible="false" />


                                <grid spacing="true">
                                    <columns>
                                        <column/>
                                        <column flex="1"/>
                                        <column/>
                                        <column flex="1"/>
                                    </columns>
                                    <rows>
                                        <row>
                                            <upload id="fileField" dropZone="dropZone" fileSizeLimit="20000000000" />
                                            <button id="clearImageBtn"
                                                    icon="font-icon:ERASER"
                                                    caption="Limpar"
                                                    invoke="onClearImageBtnClick"/>
                                        </row>
                                    </rows>
                                </grid>

                                <vbox id="dropZone"
                                      height="150px"
                                      spacing="true"
                                      stylename="dropzone-container">
                                    <label stylename="dropzone-description"
                                           value="Arraste o ficheiro aqui"
                                           icon="font-icon:UPLOAD"
                                           align="MIDDLE_CENTER"/>
                                </vbox>
                            </groupBox>
                        </column>
                    </form>
                </tab>
            </tabSheet>

        </scrollBox>
        <hbox id="editActions" spacing="true">
            <button action="windowCommitAndClose" caption="Salvar" icon="SAVE" stylename="friendly"/>
            <button action="windowClose" caption="Fechar" icon="CLOSE"/>
        </hbox>
    </layout>
</window>

```

##### Java (Funcionamento)

```java


@UiController("cmolhao_DocumentacaoCasosSociais.edit")
@UiDescriptor("documentacao-casos-sociais-edit.xml")
@EditedEntityContainer("documentacaoCasosSociaisDc")
@LoadDataBeforeShow
public class DocumentacaoCasosSociaisEdit extends StandardEditor<DocumentacaoCasosSociais> {
    @Inject
    protected InstanceContainer<DocumentacaoCasosSociais> documentacaoCasosSociaisDc;
    @Inject
    protected CollectionContainer<RedeTrabalho> redeTrabalhoDc;
    @Inject
    protected CollectionContainer<SubRedeTrabalho> subRedeTrabalhoDc;
    @Inject
    protected LookupPickerField<RedeTrabalho> idRedeTrabalhoField;
    @Inject
    protected TextField<String> numeroDocumentacaoField;
    @Inject
    protected DateField<Date> dataDocumentoField;
    @Inject
    protected TextField<String> nomeFicheiroField;
    @Inject
    protected LookupPickerField<SubRedeTrabalho> idSubRedeTrabalhoField;
    @Inject
    protected FileUploadField fileField;

    @Inject
    protected Button clearImageBtn;
    @Inject
    protected LinkButton file_name_text;
    @Inject
    protected Image file_documento;

    @Inject
    protected VBoxLayout dropZone;
    @Inject
    protected TextField<UUID> idDocumentacao;

    @Inject
    private FileUploadingAPI fileUploadingAPI;

    @Inject
    private DataManager dataManager;

    @Inject
    private Notifications notifications;

    @Inject
    private ExportDisplay exportDisplay;

    @Inject
    private Screens screens;

    public static boolean isNumeric(String str) {
        return str != null && str.matches("[-+]?\\d*\\.?\\d+");
    }

    @Subscribe
    protected void onInit(InitEvent event) {

        numeroDocumentacaoField.addValidator(value -> {
            if (value != null && !isNumeric(value))
                throw new ValidationException("O Numero de documento do ficheiro possui numeros inteiros");
        });


        fileField.addFileUploadSucceedListener(uploadSucceedEvent -> {
            FileDescriptor fd = fileField.getFileDescriptor();
            try {
                fileUploadingAPI.putFileIntoStorage(fileField.getFileId(), fd);
            } catch (FileStorageException e) {
                throw new RuntimeException("Error saving file to FileStorage", e);
            }
            getEditedEntity().setFile(dataManager.commit(fd));
            displayFicheiro();
        });

        fileField.addFileUploadErrorListener(uploadErrorEvent ->
                notifications.create()
                        .withCaption("File upload error")
                        .show());

        documentacaoCasosSociaisDc.addItemPropertyChangeListener(employeeItemPropertyChangeEvent -> {
            if ("file".equals(employeeItemPropertyChangeEvent.getProperty()))
            {
                updateImageButtons(employeeItemPropertyChangeEvent.getValue() != null);
            }
        });
    }

    private void displayFicheiro() {
        if (getEditedEntity().getFile() != null) {
            file_name_text.setCaption(getEditedEntity().getFile().getName());
            file_name_text.setVisible(true);

            String ext = getEditedEntity().getFile().getExtension();

            file_documento.setSource(ThemeResource.class)
                    .setPath("images/formats_file/file.png");

            if (ext.equals("pdf")) {
                file_documento.setSource(ThemeResource.class)
                        .setPath("images/formats_file/pdf.png");
            }
            if (ext.equals("docx")) {
                file_documento.setSource(ThemeResource.class)
                        .setPath("images/formats_file/docx.png");
            }
            if (ext.equals("doc")) {
                file_documento.setSource(ThemeResource.class)
                        .setPath("images/formats_file/doc.png");
            }
            if (ext.equals("webm")) {
                file_documento.setSource(ThemeResource.class)
                        .setPath("images/formats_file/webm.png");
            }
            if (ext.equals("mp4")) {
                file_documento.setSource(ThemeResource.class)
                        .setPath("images/formats_file/mp4.png");
            }
            if (ext.equals("mpg")) {
                file_documento.setSource(ThemeResource.class)
                        .setPath("images/formats_file/mpg.png");
            }
            if (ext.equals("ogg")) {
                file_documento.setSource(ThemeResource.class)
                        .setPath("images/formats_file/ogg.png");
            }
            if (ext.equals("avi")) {
                file_documento.setSource(ThemeResource.class)
                        .setPath("images/formats_file/avi.png");
            }
            if (ext.equals("mov")) {
                file_documento.setSource(ThemeResource.class)
                        .setPath("images/formats_file/mov.png");
            }
            if (ext.equals("wav")) {
                file_documento.setSource(ThemeResource.class)
                        .setPath("images/formats_file/wav.png");
            }
            if (ext.equals("mp3")) {
                file_documento.setSource(ThemeResource.class)
                        .setPath("images/formats_file/mp3.png");
            }
            if (ext.equals("jpg") || ext.equals("jpeg") || ext.equals("png") || ext.equals("gif")) {
                file_documento.setSource(ThemeResource.class)
                        .setPath("images/formats_file/photo.png");
            }
            if (ext.equals("rar") || ext.equals("zip")) {
                file_documento.setSource(ThemeResource.class)
                        .setPath("images/formats_file/compress.png");
            }
            if (ext.equals("xlsx")) {
                file_documento.setSource(ThemeResource.class)
                        .setPath("images/formats_file/xlsx.png");
            }
            if (ext.equals("xls")) {
                file_documento.setSource(ThemeResource.class)
                        .setPath("images/formats_file/xls.png");
            }
            if (ext.equals("csv")) {
                file_documento.setSource(ThemeResource.class)
                        .setPath("images/formats_file/csv.png");
            }
            if (ext.equals("pptx")) {
                file_documento.setSource(ThemeResource.class)
                        .setPath("images/formats_file/pptx.png");
            }
            if (ext.equals("ppt")) {
                file_documento.setSource(ThemeResource.class)
                        .setPath("images/formats_file/ppt.png");
            }

            file_documento.setVisible(true);

            dataDocumentoField.setValue(getEditedEntity().getFile().getCreateDate());
            nomeFicheiroField.setValue(getEditedEntity().getFile().getName());

        } else {
            file_name_text.setVisible(false);
            file_documento.setVisible(false);

            dataDocumentoField.setValue(null);
            nomeFicheiroField.setValue("");
        }
    }

    private void updateImageButtons(boolean enable) {
        clearImageBtn.setEnabled(enable);
    }

    public void onClearImageBtnClick() {
        getEditedEntity().setFile(null);
        displayFicheiro();
    }

    @Subscribe("file_name_text")
    protected void onFile_name_textClick(Button.ClickEvent event) {
        if (getEditedEntity().getFile() != null)
            exportDisplay.show(getEditedEntity().getFile(), ExportFormat.OCTET_STREAM);
    }


    @Subscribe
    protected void onAfterShow(AfterShowEvent event) {

        getWindow().setCaption("Adicionar/Editar Documentacao dos Casos Sociais - " + idDocumentacao.getValue());

        idRedeTrabalhoField.setEditable(false);
        Map<String, RedeTrabalho> map = new HashMap<>();
        Collection<RedeTrabalho> customers = redeTrabalhoDc.getItems();
        for (RedeTrabalho item : customers) {
            if (item.getNome().equals("Casos Sociais"))
            {
                map.put(item.getNome(), item);
                idRedeTrabalhoField.setValue(item);
            }
        }

        idRedeTrabalhoField.setOptionsMap(map);

        displayFicheiro();
        updateImageButtons(getEditedEntity().getFile() != null);


    }

    @Subscribe("idRedeTrabalhoField")
    protected void onIdRedeTrabalhoFieldValueChange(HasValue.ValueChangeEvent<RedeTrabalho> event) {
        Map<String, SubRedeTrabalho> map = new HashMap<>();
        Collection<SubRedeTrabalho> customers = subRedeTrabalhoDc.getItems();
        for (SubRedeTrabalho item : customers) {
            if (item.getIdRedeTrabalho().getId().equals(501))
            {
                if (item.getId() != null)
                {
                    map.put(item.getNome(), item);
                    idSubRedeTrabalhoField.setEditable(true);
                }


            }
        }

        idSubRedeTrabalhoField.setOptionsMap(map);
    }
}

```

##### Delete

```java
@Subscribe("documentacaoCasosSociaisesTable.remove")
    protected void onDocumentacaoCasosSociaisesTableRemove(Action.ActionPerformedEvent event) {
        documentacaoCasosSociaisesTableRemove.setConfirmation(false);
        if (documentacaoCasosSociaisesTable.getSelected().isEmpty())
        {
            dialogs.createOptionDialog()
                    .withCaption("Selecção de documentos dos casos sociais")
                    .withMessage("Deve seleccionar pelo um dos documentos dos casos sociais")
                    .withActions(
                            new DialogAction(DialogAction.Type.CLOSE)
                    )
                    .show();
        }
        else
        {
            DocumentacaoCasosSociais user = documentacaoCasosSociaisesTable.getSingleSelected();
            dialogs.createOptionDialog()
                    .withCaption("Remover a linha da tabela documento dos casos sociais número '"+user.getId()+"' ")
                    .withMessage("Tens a certeza que quer remover esta linha da tabela documento dos casos sociais número '"+user.getId()+"'?")
                    .withActions(
                            new DialogAction(DialogAction.Type.YES)
                                    .withHandler(e ->
                                    {
                                        documentacaoCasosSociaisesTableRemove.execute();
                                    }),
                            new DialogAction(DialogAction.Type.NO)
                    )
                    .show();
        }
    }
```

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------

## LOGIN

![login_accao_social](https://user-images.githubusercontent.com/9846274/204523112-d7a6468c-f0e5-4296-bd12-2387dc61646e.png)

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------

## Insituições

### Listar Instituições

![listar_instituicoes](https://user-images.githubusercontent.com/9846274/204527294-10b6796f-1c23-4803-b209-d0daf5d90a6a.png)

### Criar/Editar uma Instituição - Dados Pessoais

![create_instituicao_dados_pessoais_valencia](https://user-images.githubusercontent.com/9846274/204533931-7860a673-9c90-44a6-8133-3a91a260224c.png)

### Criar/Editar uma Instituição - Contactos e Outros

![create_instituicao_contactos_outros_valencia](https://user-images.githubusercontent.com/9846274/204534190-e9a50431-3a26-43af-9118-84cc859ffbf6.png)

### Criar/Editar uma Instituição - Morada

![create_instituicao_morada_valencia](https://user-images.githubusercontent.com/9846274/204534533-16862f46-1f2e-46d2-88a8-b0a2de0e3b4d.png)

### Criar/Editar uma Instituição - Outros Tipos de Dados / Presidencia

![create_instituicao_presidencial_valencia](https://user-images.githubusercontent.com/9846274/204535197-9b9ec4fc-f77d-4717-88fb-af8468e57fef.png)

### Criar/Editar uma Instituição - Afirmações

![create_instituicao_afirmacoes_valencia](https://user-images.githubusercontent.com/9846274/204535422-b4fcaeaf-5d2a-47f7-9140-281c8b7168c6.png)

---------------------------------------------------------------------------------------------------------------------------------------------------------------------

## Relacoes com a Instituição (Muitos para um)

![relacoes_instituicao](https://user-images.githubusercontent.com/9846274/204535721-cd1d8cbb-9063-4f26-b04d-da510e2efdc0.png)

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------

## Valênçia

### Listar Valênçia

![listar_valencias](https://user-images.githubusercontent.com/9846274/204586801-f451ebd5-96e1-41e2-b8db-cb753fdb341a.png)

### Criar/Editar Valencia através da descrição da instituição (Instituicao_id)

![criar_valencia_descricao_instituicao](https://user-images.githubusercontent.com/9846274/204540325-8fdcd004-f0aa-42af-ad89-386258aedb91.png)

### Criar/Editar Valencia - Área de Intervenção

![criar_valencia_area_intervencao_instituicao](https://user-images.githubusercontent.com/9846274/204545276-f2d9bf4a-1610-4653-92c2-82d175942ba3.png)

### Criar/Editar Valencia - Morada

![criar_valencia_morada_instituicao](https://user-images.githubusercontent.com/9846274/204545271-f004e289-3f02-4bee-960e-a1c1a192f6f4.png)

### Criar/Editar Valencia - Dados Secundários

![criar_valencia_dados_secundarios_instituicao](https://user-images.githubusercontent.com/9846274/204545886-21d8c3a9-3b47-4d0d-8d8a-3a03ad845659.png)

### Criar/Editar Valencia - Acordo Capacidade (Datas)

![criar_valencia_acordo_capacidade_instituicao](https://user-images.githubusercontent.com/9846274/204552221-fb033b86-9fa6-4897-b3fd-f58f099196da.png)

### Criar/Editar Valencia - Acordo Utentes

![criar_valencia_acordo_utentes_instituicao](https://user-images.githubusercontent.com/9846274/204587007-125c20b9-c641-4039-a772-ad32ae4384c3.png)

### Criar/Editar Valencia - Contratos

![criar_valencia_contratos_instituicao](https://user-images.githubusercontent.com/9846274/204586989-dfdbe058-c8e0-48f3-b355-44d2b77eb802.png)

### Criar/Editar Valencia - Certidão

![criar_valencia_certidao_instituicao](https://user-images.githubusercontent.com/9846274/204586994-6a9e70dd-4496-4665-95cd-ad8266420e5e.png)

### Criar/Editar Valencia - Atividades

![criar_valencia_atividades_instituicao](https://user-images.githubusercontent.com/9846274/204586998-3af89918-574f-4403-b725-ba7867b3d2be.png)

### Criar/Editar Valencia - Contratos

![criar_valencia_acordo_vagas_instituicao](https://user-images.githubusercontent.com/9846274/204587003-cba678aa-ea0a-413c-8bf4-bf7419c461b3.png)

### Criar/Editar Valencia - Transportes

![criar_valencia_transportes_instituicao](https://user-images.githubusercontent.com/9846274/204587012-9ad5dc46-b494-4384-977e-b88b1d0e3a7b.png)

### Criar/Editar Valencia - Recursos de Serviços

![criar_valencia_recursos_servicos_instituicao](https://user-images.githubusercontent.com/9846274/204587013-dd4ac832-6fc8-4996-8874-a5fe1d2393e8.png)

### Criar/Editar Valencia - Horários

![criar_valencia_horarios_instituicao](https://user-images.githubusercontent.com/9846274/204587016-5d5fb394-9ca1-46e7-b75e-c4eb097f0fef.png)

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------

## Relacoes com a Valênçia (Muitos para um)

![relacoes_valencias](https://user-images.githubusercontent.com/9846274/204592005-0f47011d-b008-4635-b295-eb0d46dbba1c.png)

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------

## Projetos / Outras Atividades

### Listar Projetos / Outras Atividades

![listar_projetos_intervencao](https://user-images.githubusercontent.com/9846274/204596570-d56bf70a-3dc9-43d0-a9e0-fabb977906f3.png)

### Criar/Editar Projetos / Outras Atividades - Dados do Projeto

![criar_editar_dados_projetos_projetos_intervencao](https://user-images.githubusercontent.com/9846274/204597023-e97fc995-1a08-40eb-a6fe-f684f73c579f.png)

### Criar/Editar Projetos / Outras Atividades - Adultos e Deficientes

![criar_editar_adultos_deficientes_projetos_intervencao](https://user-images.githubusercontent.com/9846274/204601264-f9e0ad4d-f581-4731-8882-ce98280ddabf.png)

### Criar/Editar Projetos / Outras Atividades - Crianças e Jovens

![criar_editar_dados_projetos_projetos_intervencao](https://user-images.githubusercontent.com/9846274/204600992-ad73e025-a809-49a7-87a3-cee6a83645d9.png)

### Criar/Editar Projetos / Outras Atividades - Idosos

![criar_editar_idosos_projetos_intervencao](https://user-images.githubusercontent.com/9846274/204600918-19a8b15e-c995-4e40-b66b-cb28d479ccc0.png)

### Criar/Editar Projetos / Outras Atividades - Outros Serviços

![criar_editar_outros_servicos_projetos_intervencao](https://user-images.githubusercontent.com/9846274/204601291-64e2ec4f-924e-4d8c-8364-6e832d78795c.png)

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------

## Relacoes com Projetos de Intervenção (Muitos para um)

![relacoes_projetos_intervencai](https://user-images.githubusercontent.com/9846274/204600752-9fe425a0-048c-4328-83cf-525ef713ef46.png)

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------

## Claso 

### Listagem de Membros de Instituição

![listar_membros_rede_claso](https://user-images.githubusercontent.com/9846274/204643131-860b29ed-cd86-4209-912f-3b4148675357.png)

### Editar/Criar Membros da Instituição (Claso)

![criar_editar_membros_instituicao_claso](https://user-images.githubusercontent.com/9846274/204753877-8d449a24-d700-4c60-a73e-04678a506216.png)

### Listagem de Sub Rede de Trabalho - Atas

![listar_membros_rede_sub_rede_atas](https://user-images.githubusercontent.com/9846274/204643127-c5b944d2-3629-4a32-9699-ab4edb7eb69c.png)

### Editar/Criar Membros da Instituição (Claso) da Sub Rede de Trabalho - Atas, Fichas de Adesão, Regulamentos (Dados Pessoais)

![criar_editar_membros_instituicao_claso_sub_rede_atas_dados_pessoais](https://user-images.githubusercontent.com/9846274/204755454-0ae643e7-1818-4a27-9e56-cdef489b522c.png)

### Editar/Criar Membros da Instituição (Claso) da Sub Rede de Trabalho - Atas, Fichas de Adesão, Regulamentos (Ficheiros)

![criar_editar_membros_instituicao_claso_sub_rede_atas_ficheiro](https://user-images.githubusercontent.com/9846274/204756371-23f1f959-8335-46fa-8bde-0f505647614f.png)

### Listagem de Sub Rede de Trabalho - Fichas de Adesão

![listar_membros_rede_sub_rede_fichas](https://user-images.githubusercontent.com/9846274/204643135-970dec77-e4a1-4a10-8652-2fb3189810a1.png)

### Listagem de Sub Rede de Trabalho - Regulamentos

![listar_membros_rede_sub_rede_regulametos](https://user-images.githubusercontent.com/9846274/204643145-e29f970f-833f-46a9-a3d7-8d248ea86a94.png)

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------

## Nucleo Executivo

### Listagem de Membros de Instituição

![listar_membros_rede_nucleoexecutivo](https://user-images.githubusercontent.com/9846274/204758656-bb1cc784-2793-4af3-bff2-a9d23b281f77.png)

### Editar/Criar Membros da Instituição (Nucleo Executivo)

![criar_editar_membros_instituicao_nucleoexecutivo](https://user-images.githubusercontent.com/9846274/204759077-416f613d-2385-45ee-8aad-ca77b536862c.png)

### Listagem de Sub Rede de Trabalho - Fichas de Adesão

![listar_membros_rede_sub_rede_atas_nucleoexecutivo](https://user-images.githubusercontent.com/9846274/204761934-0edebc9d-6180-43fe-b3e9-549b841146b9.png)

### Editar/Criar Membros da Instituição (Nucleo Executivo) da Sub Rede de Trabalho - Atas (Dados Pessoais)

![criar_editar_membros_instituicao_nucleoexecutivo_sub_rede_atas_dados_pessoais](https://user-images.githubusercontent.com/9846274/204762660-55e0ea7e-59cc-4b29-b75f-1723a7b303c2.png)

### Editar/Criar Membros da Instituição (Nucleo Executivo) da Sub Rede de Trabalho - Atas (Ficheiros)

![criar_editar_membros_instituicao_nucleoexecutivo_sub_rede_atas_ficheiro](https://user-images.githubusercontent.com/9846274/204762663-15ed7b20-6014-41c7-b991-70b84ae2cacd.png)

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------

## Grupos de Trabalho

### Listagem de Rede "Grupos de Trabalho" 

![listar_membros_rede_grupostrabalho](https://user-images.githubusercontent.com/9846274/204764203-a4f3acbc-ddde-4461-81ca-d1b0abafcb1b.png)

### Editar/Criar Membros da Instituição (Nucleo Executivo) da Sub Rede de Trabalho (Dados Pessoais)

![criar_editar_membros_rede_nucleoexecutivo_dados_pessoais](https://user-images.githubusercontent.com/9846274/204839923-d8c8038b-4483-4b5c-a1f9-af5f77d62c9f.png)

### Editar/Criar Membros da Instituição (Nucleo Executivo) da Sub Rede de Trabalho (Ficheiro)

![criar_editar_membros_rede_nucleoexecutivo_ficheiro](https://user-images.githubusercontent.com/9846274/204839929-991a3d3b-1a0b-40f4-9f16-cee0ebff4d72.png)

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------

## Plataforma Supraconcelhia

### Listagem de Rede "Plataforma Supraconcelhia"

![listar_membros_rede_plataforma](https://user-images.githubusercontent.com/9846274/204843018-29b1864e-36fa-405c-b550-70d6584680df.png)

### Editar/Criar Membros da Instituição (Plataforma Supraconcelhia) da Sub Rede de Trabalho (Dados Pessoais)

![criar_editar_membros_rede_plataforma_dados_pessoais](https://user-images.githubusercontent.com/9846274/204844269-05b16add-6495-47ac-940a-4929a1757476.png)

### Editar/Criar Membros da Instituição (Plataforma Supraconcelhia) da Sub Rede de Trabalho (Ficheiro)

![criar_editar_membros_rede_plataforma_ficheiro](https://user-images.githubusercontent.com/9846274/204844274-1918ad51-5992-4714-b31c-936e62d4c6c8.png)

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------

## Instrumentos de Planeamento

### Listagem de Membros de Instituição (Diagnostico Social & Plano de Desenvolvimento)

![listar_membros_rede_sub_rede_diagnosticosocial_instrumentosdeplan](https://user-images.githubusercontent.com/9846274/204846126-4f010ac4-cad8-49d3-969d-4952bdfb6680.png)

### Editar/Criar Membros da Instituição (Instrumentos de Planeamento) da Sub Rede de Trabalho - Diagnostico Social / Plano de Desenvolvimento (Dados Pessoais)

![criar_editar_membros_rede_sub_rede_diagnosticosocial_instrumentosdeplan_dados_pessoais](https://user-images.githubusercontent.com/9846274/204848053-15863094-99d0-40d1-beab-40904643e2ed.png)

### Editar/Criar Membros da Instituição (Instrumentos de Planeamento) da Sub Rede de Trabalho - Diagnostico Social / Plano de Desenvolvimento (Ficheiros)

![criar_editar_membros_rede_sub_rede_diagnosticosocial_instrumentosdeplan_ficheiros](https://user-images.githubusercontent.com/9846274/204848059-2687c67b-21bd-4203-9e04-31177ef1d3be.png)

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------

## Documentação

### Listagem de Membros de Instituição (Legislação, Regulamentos, Minutas)

![listar_membros_rede_documentacao_sub_rede_legislacao](https://user-images.githubusercontent.com/9846274/204867256-475c7c36-3bbb-4fd0-b8ec-5b1cb047145d.png)

### Editar/Criar Membros da Instituição das Sub Redes de Trabalho da Legislação, Regulamentos, Minutas (Dados Pessoais)

![create_edit_membros_rede_documentacao_sub_rede_legislacao_dados_pessoais](https://user-images.githubusercontent.com/9846274/204867247-b1005b9f-f5d9-4f40-97b9-f4d81ba322d8.png)

### Editar/Criar Membros da Instituição das Sub Redes de Trabalho da Legislação, Regulamentos, Minutas (Ficheiros)

![create_edit_membros_rede_documentacao_sub_rede_legislacao_ficheiro](https://user-images.githubusercontent.com/9846274/204867254-2c1049b8-2d9e-4e50-91c1-a1a64fffdb81.png)

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------

## Documentação na Habitação Social

### Listagem de Habitação Social - Documentação 

![listar_habitacao_social](https://user-images.githubusercontent.com/9846274/204867765-e29fd4fa-ccad-4add-97da-2267e0d81315.png)

### Editar/Criar Habitação Social - Dados Pessoais

![criar_editar_habitacao_social_dados_pessoais](https://user-images.githubusercontent.com/9846274/205278381-01d1478b-1d4e-4d64-a3da-b71a8663ed17.png)

### Editar/Criar Habitação Social - Ficheiros

![criar_editar_habitacao_social_ficheiro](https://user-images.githubusercontent.com/9846274/205278376-3013aa72-e67a-46f8-ba57-87e25c9286f5.png)

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------

## Programa de Arrendamento de Apoio - Habitação Social

### Listagem de Programa de Arrendamento de Apoio - Habitação Social (1ª Fase)

![listar_programa_apoio_1_fase_habitacai_social](https://user-images.githubusercontent.com/9846274/205278584-499e291c-c9b9-420b-a58e-e403f382d760.png)

### Criar/Editar do Programa de Arrendamento de Apoio - Habitação Social (1ª Fase) - Dados Pessoais

![criar_editar_programa_apoio_1_fase_habitacai_social_dadospessoais](https://user-images.githubusercontent.com/9846274/205281360-8063ac21-85a3-40fb-af1f-49d0680c8289.png)

### Listagem de Programa de Arrendamento de Apoio - Habitação Social (2ª Fase)

![listar_programa_apoio_2_fase_habitacai_social](https://user-images.githubusercontent.com/9846274/205281709-404112d8-f34c-4420-958e-5e7256ebe8f5.png)

### Criar/Editar do Programa de Arrendamento de Apoio - Habitação Social (2ª Fase) - Dados Pessoais

![criar_editar_programa_apoio_2_fase_habitacai_social_dadospessoais](https://user-images.githubusercontent.com/9846274/205304771-21efddb1-0f4c-42e0-a74c-ecb5956563a7.png)


### Listagem de Programa de Arrendamento de Apoio - Habitação Social (3ª Fase)

![listar_programa_apoio_3_fase_habitacai_social](https://user-images.githubusercontent.com/9846274/205304966-52f5718f-5822-425a-aa17-d5d6b00f337d.png)

### Criar/Editar do Programa de Arrendamento de Apoio - Habitação Social (3ª Fase) - Dados Pessoais

![criar_editar_programa_apoio_3_fase_habitacai_social_dadospessoais](https://user-images.githubusercontent.com/9846274/205304994-499b08ef-cef7-48c3-9d81-0bbc8a08b876.png)

### Listagem de Programa de Arrendamento de Apoio - Habitação Social (Documentação)

![listar_programa_apoio_documentacao_habitacai_social](https://user-images.githubusercontent.com/9846274/205305499-37008af0-6091-4a6c-8456-38b26f0fba9d.png)

### Criar/Editar do Programa de Arrendamento de Apoio - Habitação Social (Documentação) - Dados Pessoais

![criar_editar_programa_apoio_documentacao_habitacai_social](https://user-images.githubusercontent.com/9846274/205305488-d0710709-ec4f-4bed-8297-4e7521f30e51.png)

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------

## Casos Sociais - Rede de Trabalho

### Listagem de Membros de Instituição na rede de trabalho: Casos Sociais

![listar_membros_rede_trabalho_casossociais](https://user-images.githubusercontent.com/9846274/205334295-59b16763-5e81-42ac-b244-83a77d705641.png)

### Editar/Criar Membros de Instituição na rede de trabalho: Casos Sociais (Dados Pessoais)

