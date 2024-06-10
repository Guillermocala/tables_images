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
    Packer,
    PageOrientation,
    Paragraph,
    SectionType,
    Table,
    TableCell,
    TableRow,
    TextRun,
    ImageRun,
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
        const generate_docx = async (firstSection) => {
            let childrens = [];

            const urls = [
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
                'https://raw.githubusercontent.com/dolanmiu/docx/ccd655ef8be3828f2c4b1feb3517a905f98409d9/demo/images/cat.jpg',
            ];

            let imageData;

            try {
                const imagePromises = urls.map(url => axios.get(url, {
                    responseType: 'blob'
                }));
                const imageBlobs = await Promise.all(imagePromises);


                /* imageData = imageBlobs.map(blob => ({
                    data: URL.createObjectURL(blob)
                })); // Create data URLs */

                console.log(imageBlobs);
                imageData = imageBlobs;
            } catch (error) {
                console.error('An error occurred fetching images:', error);
            }

            for (let index = 0; index < urls.length; index++) {
                childrens.push(
                    new TableRow({
                        children: [
                            new TableCell({
                                /* width: {
                                      size: 30,
                                      type: WidthType.PERCENTAGE,
                                }, */
                                children: [
                                    new Paragraph({
                                        text: "Maq- Eq. Optometría: " + index,
                                        style: "globalPar",
                                    }),
                                ],
                            }),
                            new TableCell({
                                /* width: {
                                      size: 70,
                                      type: WidthType.PERCENTAGE,
                                }, */
                                children: [
                                    new Paragraph({
                                        text: "400000005: " + index,
                                        style: "globalPar",
                                    }),
                                ],
                            }),
                            new TableCell({
                                /* width: {
                                      size: 70,
                                      type: WidthType.PERCENTAGE,
                                }, */
                                children: [
                                    new Paragraph({
                                        text: ": " + index,
                                        style: "globalPar",
                                    }),
                                ],
                            }),
                            new TableCell({
                                /* width: {
                                      size: 70,
                                      type: WidthType.PERCENTAGE,
                                }, */
                                children: [
                                    new Paragraph({
                                        text: "Mesa eléctrica: " + index,
                                        style: "globalPar",
                                    }),
                                ],
                            }),
                            new TableCell({
                                /* width: {
                                      size: 70,
                                      type: WidthType.PERCENTAGE,
                                }, */
                                children: [
                                    new Paragraph({
                                        text: "Faltante: " + index,
                                        style: "globalPar",
                                    }),
                                ],
                            }),
                            new TableCell({
                                /* width: {
                                      size: 70,
                                      type: WidthType.PERCENTAGE,
                                }, */
                                children: [
                                    new Paragraph({
                                        children: [
                                            new ImageRun({
                                                data: imageData[index].data,
                                                transformation: {
                                                    width: 100,
                                                    height: 100,
                                                },
                                            })
                                        ],
                                    }),
                                ],
                            }),
                        ],
                    }),
                );
            }

            const dataSection = {
                properties: {
                    type: SectionType.CONTINUOUS,
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
                                        /* width: {
                                              size: 30,
                                              type: WidthType.PERCENTAGE,
                                        }, */
                                        children: [
                                            new Paragraph({
                                                text: "Tipo de activo",
                                                style: "globalPar",
                                            }),
                                        ],
                                    }),
                                    new TableCell({
                                        /* width: {
                                              size: 70,
                                              type: WidthType.PERCENTAGE,
                                        }, */
                                        children: [
                                            new Paragraph({
                                                text: "SAP",
                                                style: "globalPar",
                                            }),
                                        ],
                                    }),
                                    new TableCell({
                                        /* width: {
                                              size: 70,
                                              type: WidthType.PERCENTAGE,
                                        }, */
                                        children: [
                                            new Paragraph({
                                                text: "Código",
                                                style: "globalPar",
                                            }),
                                        ],
                                    }),
                                    new TableCell({
                                        /* width: {
                                              size: 70,
                                              type: WidthType.PERCENTAGE,
                                        }, */
                                        children: [
                                            new Paragraph({
                                                text: "Descripción",
                                                style: "globalPar",
                                            }),
                                        ],
                                    }),
                                    new TableCell({
                                        /* width: {
                                              size: 70,
                                              type: WidthType.PERCENTAGE,
                                        }, */
                                        children: [
                                            new Paragraph({
                                                text: "Estado",
                                                style: "globalPar",
                                            }),
                                        ],
                                    }),
                                    new TableCell({
                                        /* width: {
                                              size: 70,
                                              type: WidthType.PERCENTAGE,
                                        }, */
                                        children: [
                                            new Paragraph({
                                                text: "Foto",
                                                style: "globalPar",
                                            }),
                                        ],
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

            const doc = new Document({
                sections: [firstSection, dataSection],
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
                            font: "Arial",
                        },
                        paragraph: {
                            alignment: AlignmentType.JUSTIFIED,
                        },
                    }, ],
                    characterStyles: [{
                        id: "globalChar",
                        name: "Global",
                        basedOn: "Normal",
                        next: "Normal",
                        run: {
                            size: 24,
                            font: "Arial",
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
            generate_docx(firstSection);
        };

        return {
            generate_docx,
            generar,
        };
    },
};
</script>
