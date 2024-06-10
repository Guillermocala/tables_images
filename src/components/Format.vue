<template>
<button @click="generar()">Click Me!</button>
</template>

<script>
import {
    saveAs
} from "file-saver";
import {
    AlignmentType,
    convertMillimetersToTwip,
    Document,
    ImageRun,
    Packer,
    PageOrientation,
    Paragraph,
    SectionType,
    Table,
    TableCell,
    TableRow,
    TextRun,
    WidthType,
} from "docx";
import axios from 'axios';

export default {
    name: "DocxJS",
    props: {
        temporal: {
            type: String,
            default: "soy un prop temporal",
        },
    },

    setup() {
        const fetchImages = async (urls) => {
            try {
                const imagePromises = urls.map(url => axios.get(url, {
                    responseType: 'blob'
                }));
                const imageBlobs = await Promise.all(imagePromises);

                return imageBlobs;
            } catch (error) {
                console.error('An error occurred fetching images:', error);
            }
        };

        const createTableRow = (imageData) => {
            return new TableRow({
                children: [
                    new TableCell({
                        children: [
                            new Paragraph({
                                text: "Maq- Eq. Optometría",
                                style: "globalPar"
                            }),
                        ],
                    }),
                    new TableCell({
                        children: [
                            new Paragraph({
                                text: "400000005",
                                style: "globalPar"
                            }),
                        ],
                    }),
                    new TableCell({
                        children: [
                            new Paragraph({
                                text: "",
                                style: "globalPar"
                            }),
                        ],
                    }),
                    new TableCell({
                        children: [
                            new Paragraph({
                                text: "Mesa eléctrica",
                                style: "globalPar"
                            }),
                        ],
                    }),
                    new TableCell({
                        children: [
                            new Paragraph({
                                text: "Faltante",
                                style: "globalPar"
                            }),
                        ],
                    }),
                    new TableCell({
                        children: [
                            new Paragraph({
                                children: [
                                    new ImageRun({
                                        data: imageData.data,
                                        transformation: {
                                            width: 100,
                                            height: 100,
                                        },
                                    }),
                                ],
                            }),
                        ],
                    }),
                ],
            });
        };

        const createDataSection = (childrens) => {
            return {
                properties: {
                    type: SectionType.CONTINUOUS
                },
                children: [
                    new Table({
                        margins: {
                            top: convertMillimetersToTwip(1),
                            right: convertMillimetersToTwip(2),
                            bottom: convertMillimetersToTwip(1),
                            left: convertMillimetersToTwip(2),
                        },
                        rows: [
                            new TableRow({
                                children: [
                                    new TableCell({
                                        children: [new Paragraph({
                                            text: "Tipo de activo",
                                            style: "globalPar"
                                        })]
                                    }),
                                    new TableCell({
                                        children: [new Paragraph({
                                            text: "SAP",
                                            style: "globalPar"
                                        })]
                                    }),
                                    new TableCell({
                                        children: [new Paragraph({
                                            text: "Código",
                                            style: "globalPar"
                                        })]
                                    }),
                                    new TableCell({
                                        children: [new Paragraph({
                                            text: "Descripción",
                                            style: "globalPar"
                                        })]
                                    }),
                                    new TableCell({
                                        children: [new Paragraph({
                                            text: "Estado",
                                            style: "globalPar"
                                        })]
                                    }),
                                    new TableCell({
                                        children: [new Paragraph({
                                            text: "Foto",
                                            style: "globalPar"
                                        })]
                                    }),
                                ],
                            }),
                            ...childrens,
                        ],
                        width: {
                            size: 100,
                            type: WidthType.PERCENTAGE,
                        },
                    }),
                ],
            };
        };

        const createDocument = async (firstSection, childrens) => {
            const doc = new Document({
                sections: [firstSection, createDataSection(childrens)],
                size: {
                    orientation: PageOrientation.LANDSCAPE,
                },
                styles: {
                    paragraphStyles: [{
                        id: "globalPar",
                        name: "Global",
                        basedOn: "Normal",
                        next: "Normal",
                        run: {
                            size: 24,
                            font: "Arial"
                        },
                        paragraph: {
                            alignment: AlignmentType.JUSTIFIED
                        },
                    }, ],
                    characterStyles: [{
                        id: "globalChar",
                        name: "Global",
                        basedOn: "Normal",
                        next: "Normal",
                        run: {
                            size: 24,
                            font: "Arial"
                        },
                    }, ],
                },
            });

            try {
                const blob = await Packer.toBlob(doc);
                saveAs(blob, "fichero.docx");
            } catch (error) {
                console.log(error);
            }
        };

        const generateDocx = async (firstSection) => {
            const urls = [
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
            ];

            const imageData = await fetchImages(urls);
            console.log("imageData", imageData);
            const childrens = urls.map((url, index) => createTableRow(imageData[index]));
            await createDocument(firstSection, childrens);
        };

        const generar = async () => {
            const firstSection = {
                properties: {
                    type: SectionType.CONTINUOUS,
                },
                children: [
                    new Paragraph({
                        children: [
                            new TextRun({
                                text: "Inventario",
                                bold: true,
                                style: "globalChar",
                            }),
                        ],
                        alignment: AlignmentType.CENTER,
                        spacing: {
                            after: 400,
                        },
                    }),
                ],
            };
            await generateDocx(firstSection);
        };

        return {
            generar,
        };
    },
};
</script>
