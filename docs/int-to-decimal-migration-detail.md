# Int → Decimal Migration Detail

> **Issue:** [#1 Change Integer quantity fields to BigDecimal](https://github.com/agusprp/wms-openboxes/issues/1)
> **Branch:** `feature/int-to-decimal-quantity`
> **Commits:** `04e5ee09d` + `4884ad8e2` + `cd4ec722d`

## Ringkasan

Mengubah semua field quantity dari `Integer` menjadi `BigDecimal` (DB: `int(11)` → `decimal(19,3)`) di seluruh domain, service, dan controller OpenBoxes. Total **52 files** diubah (+ 3 commit follow-up fixes).

---

## A. Domain Files (29 files, ~130 field/method changes)

### 1. `FulfillmentItem.groovy`
- **Lokasi:** `grails-app/domain/org/pih/warehouse/fulfillment/FulfillmentItem.groovy`
- **Fungsi file:** Fulfillment/pemenuhan order — item dalam fulfillment
- **Field diubah:**
  | Field/Method | Line (baru) | Tipe Lama | Tipe Baru |
  |---|---|---|---|
  | `quantity` | 20 | `Integer` | `BigDecimal` |

### 2. `CycleCountCandidate.groovy`
- **Lokasi:** `grails-app/domain/org/pih/warehouse/inventory/CycleCountCandidate.groovy`
- **Fungsi file:** Kandidat untuk cycle count — produk yang harus dihitung
- **Field diubah:**
  | Field | Line (baru) | Tipe Lama | Tipe Baru |
  |---|---|---|---|
  | `quantityOnHand` | 25 | `Integer` | `BigDecimal` |
  | `quantityAllocated` | 27 | `Integer` | `BigDecimal` |

### 3. `CycleCountDetails.groovy`
- **Lokasi:** `grails-app/domain/org/pih/warehouse/inventory/CycleCountDetails.groovy`
- **Fungsi file:** Detail hasil cycle count — blind count + verification count
- **Field/method diubah:**
  | Field/Method | Line (baru) | Tipe Lama | Tipe Baru |
  |---|---|---|---|
  | `blindCountQuantityOnHand` | 28 | `Integer` | `BigDecimal` |
  | `blindCountQuantityCounted` | 29 | `Integer` | `BigDecimal` |
  | `blindCountQuantityVariance` | 30 | `Integer` | `BigDecimal` |
  | `verificationCountQuantityOnHand` | 36 | `Integer` | `BigDecimal` |
  | `verificationCountQuantityCounted` | 37 | `Integer` | `BigDecimal` |
  | `verificationCountQuantityVariance` | 38 | `Integer` | `BigDecimal` |
  | `getVarianceTypeCode()` param `quantityVariance` | 54 | `Integer` | `BigDecimal` |

### 4. `CycleCountItem.groovy`
- **Lokasi:** `grails-app/domain/org/pih/warehouse/inventory/CycleCountItem.groovy`
- **Fungsi file:** Item dalam cycle count
- **Field/method diubah:**
  | Field/Method | Line (baru) | Tipe Lama | Tipe Baru |
  |---|---|---|---|
  | `quantityOnHand` | 30 | `Integer` | `BigDecimal` |
  | `quantityCounted` | 32 | `Integer` | `BigDecimal` |
  | `getQuantityVariance()` | 109 | `Integer` → `BigDecimal` | return type |

### 5. `CycleCountSummary.groovy`
- **Lokasi:** `grails-app/domain/org/pih/warehouse/inventory/CycleCountSummary.groovy`
- **Fungsi file:** Ringkasan hasil cycle count
- **Field/method diubah:** 6 fields + 1 method parameter `quantityVariance` (line 50)

### 6. `InventoryAuditDetails.groovy`
- **Lokasi:** `grails-app/domain/org/pih/warehouse/inventory/InventoryAuditDetails.groovy`
- **Fungsi file:** Detail audit inventory
- **Field diubah:**
  | Field | Line (baru) | Tipe Lama | Tipe Baru |
  |---|---|---|---|
  | `quantityAdjusted` | 22 | `Integer` | `BigDecimal` |
  | `quantityOnHand` | 23 | `Integer` | `BigDecimal` |

### 7. `InventoryAuditSummary.groovy`
- **Lokasi:** `grails-app/domain/org/pih/warehouse/inventory/InventoryAuditSummary.groovy`
- **Fungsi file:** Ringkasan audit inventory
- **Field diubah:**
  | Field | Line (baru) | Tipe Lama | Tipe Baru |
  |---|---|---|---|
  | `quantityAdjusted` | 11 | `Integer = 0` | `BigDecimal = BigDecimal.ZERO` |
  | `quantityDemanded` | 16 | `Integer = 0` | `BigDecimal = BigDecimal.ZERO` |
  | `quantityOnHand` | 17 | `Integer = 0` | `BigDecimal = BigDecimal.ZERO` |

### 8. `InventoryItem.groovy`
- **Lokasi:** `grails-app/domain/org/pih/warehouse/inventory/InventoryItem.groovy`
- **Fungsi file:** Item inventory — track lot/batch product + quantity
- **Field diubah:**
  | Field | Line (baru) | Tipe Lama | Tipe Baru |
  |---|---|---|---|
  | `quantity` | 52 | `Integer` | `BigDecimal` |
  | `quantityOnHand` | 53 | `Integer` | `BigDecimal` |
  | `quantityAvailableToPromise` | 54 | `Integer` | `BigDecimal` |

### 9. `InventoryItemSnapshot.groovy`
- **Lokasi:** `grails-app/domain/org/pih/warehouse/inventory/InventoryItemSnapshot.groovy`
- **Fungsi file:** Snapshot inventory per item (historical)
- **Field diubah:**
  | Field | Line (baru) | Tipe Lama | Tipe Baru |
  |---|---|---|---|
  | `quantityOnHand` | 26 | `Integer` | `BigDecimal` |
  | `quantityInbound` | 27 | `Integer` | `BigDecimal` |
  | `quantityOutbound` | 28 | `Integer` | `BigDecimal` |

### 10. `InventoryLevel.groovy`
- **Lokasi:** `grails-app/domain/org/pih/warehouse/inventory/InventoryLevel.groovy`
- **Fungsi file:** Level inventory — min, reorder, max quantity per product
- **Field/method diubah:**
  | Field/Method | Line (baru) | Tipe Lama | Tipe Baru |
  |---|---|---|---|
  | `minQuantity` | 35 | `Integer` | `BigDecimal` |
  | `reorderQuantity` | 38 | `Integer` | `BigDecimal` |
  | `maxQuantity` | 41 | `Integer` | `BigDecimal` |
  | `getMonthlyForecastQuantity()` | 121 | `Integer` → `BigDecimal` | return type |

### 11. `InventorySnapshot.groovy`
- **Lokasi:** `grails-app/domain/org/pih/warehouse/inventory/InventorySnapshot.groovy`
- **Fungsi file:** Snapshot inventory agregat per location
- **Field diubah:**
  | Field | Line (baru) | Tipe Lama | Tipe Baru |
  |---|---|---|---|
  | `quantityOnHand` | 31 | `Integer` | `BigDecimal` |
  | `quantityInbound` | 32 | `Integer` | `BigDecimal` |
  | `quantityOutbound` | 33 | `Integer` | `BigDecimal` |

### 12. `PendingCycleCountRequest.groovy`
- **Lokasi:** `grails-app/domain/org/pih/warehouse/inventory/PendingCycleCountRequest.groovy`
- **Fungsi file:** Request cycle count yang pending
- **Field diubah:**
  | Field | Line (baru) | Tipe Lama | Tipe Baru |
  |---|---|---|---|
  | `quantityOnHand` | 35 | `Integer` | `BigDecimal` |
  | `quantityAllocated` | 37 | `Integer` | `BigDecimal` |

### 13. `Requirement.groovy`
- **Lokasi:** `grails-app/domain/org/pih/warehouse/inventory/Requirement.groovy`
- **Fungsi file:** Kebutuhan replenishment product
- **Field diubah:**
  | Field | Line (baru) | Tipe Lama | Tipe Baru |
  |---|---|---|---|
  | `quantityInBin` | 11 | `Integer` | `BigDecimal` |
  | `minQuantity` | 12 | `Integer` | `BigDecimal` |
  | `maxQuantity` | 13 | `Integer` | `BigDecimal` |
  | `reorderQuantity` | 14 | `Integer` | `BigDecimal` |
  | `totalQuantityAvailableToPromise` | 15 | `Integer` | `BigDecimal` |
  | `quantityAvailable` | 16 | `Integer` | `BigDecimal` |

### 14. `TransactionEntry.groovy`
- **Lokasi:** `grails-app/domain/org/pih/warehouse/inventory/TransactionEntry.groovy`
- **Fungsi file:** Entry transaksi inventory — setiap perubahan stok
- **Field diubah:**
  | Field | Line (baru) | Tipe Lama | Tipe Baru |
  |---|---|---|---|
  | `quantity` | 20 | `Integer` | `BigDecimal` |

### 15. `InvoiceItem.groovy`
- **Lokasi:** `grails-app/domain/org/pih/warehouse/invoice/InvoiceItem.groovy`
- **Fungsi file:** Item dalam invoice
- **Field/method diubah:**
  | Field/Method | Line (baru) | Tipe Lama | Tipe Baru |
  |---|---|---|---|
  | `quantity` | 43 | `Integer` | `BigDecimal` |
  | `validator` param `quantity` | 83 | `Integer` | `BigDecimal` |
  | `originalQuantityInvoiced` (local) | 95 | `Integer` | `BigDecimal` |
  | `quantityInvoicedOutside` (local) | 99 | `Integer` | `BigDecimal` |
  | `quantityShippedInUom` (local) | 101 | `Integer` | `BigDecimal` |
  | `quantityAvailableToInvoice` (local) | 104 | `Integer` | `BigDecimal` |
  | `getQuantityAvailableToInvoice()` | 165 | `Integer` → `BigDecimal` | return type |

### 16. `InvoiceItemCandidate.groovy`
- **Lokasi:** `grails-app/domain/org/pih/warehouse/invoice/InvoiceItemCandidate.groovy`
- **Fungsi file:** Kandidat item invoice — quantity dari order/shipment
- **Field diubah:**
  | Field | Line (baru) | Tipe Lama | Tipe Baru |
  |---|---|---|---|
  | `quantity` | 38 | `Integer` | `BigDecimal` |
  | `orderItemQuantity` | 39 | `Integer` | `BigDecimal` |
  | `quantityToInvoice` | 40 | `Integer` | `BigDecimal` |

### 17. `OrderItem.groovy` ⭐ (paling banyak berubah)
- **Lokasi:** `grails-app/domain/org/pih/warehouse/order/OrderItem.groovy`
- **Fungsi file:** Item dalam purchase order — inti transaksi pembelian
- **Field/method diubah:**
  | Field/Method | Line (baru) | Tipe Lama | Tipe Baru | Keterangan |
  |---|---|---|---|---|
  | `quantity` | 49 | `Integer` | `BigDecimal` | Field inti — qty yang dipesan |
  | `getQuantityInStandardUom()` | 199 | `Integer` | `BigDecimal` | Qty dalam standard UOM |
  | `getQuantityShippedInStandardUom()` | 203 | `Integer` | `BigDecimal` | Qty shipped dalam std UOM |
  | `getQuantityInShipmentsInStandardUom()` | 209 | `Integer` | `BigDecimal` | Qty di shipment |
  | `getQuantityReceivedInStandardUom()` | 215 | `Integer` | `BigDecimal` | Qty diterima |
  | `getQuantityCanceledInStandardUom()` | 221 | `Integer` | `BigDecimal` | Qty dibatalkan |
  | `getQuantityShipped()` | 227 | `Integer` | `BigDecimal` | Total qty shipped |
  | `getQuantityReceived()` | 231 | `Integer` | `BigDecimal` | Total qty received |
  | `getQuantityCanceled()` | 235 | `Integer` | `BigDecimal` | Total qty canceled |
  | `getQuantityInShipments()` | 239 | `Integer` | `BigDecimal` | Qty in transit |
  | `getQuantityRemainingToShip(Shipment)` | 247 | `Integer` | `BigDecimal` | Sisa qty to ship |
  | `getQuantityRemaining()` | 255 | `Integer` | `BigDecimal` | Sisa qty umum |
  | `getQuantityInvoiced()` | 307 | `Integer` | `BigDecimal` | Qty sudah di-invoice |
  | `getQuantityInvoicedInStandardUom()` | 314 | `Integer` | `BigDecimal` | Qty invoice std UOM |
  | `getQuantityAvailableToInvoice()` | 318 | `Integer` | `BigDecimal` | Qty sisa invoice |
  | `getPostedQuantityInvoicedInStandardUom()` | 418 | `Integer` | `BigDecimal` | Qty invoice posted |
  | `getPostedQuantityInvoiced()` | 438 | `Integer` | `BigDecimal` | Qty invoice posted local |

### 18. `OrderItemDetails.groovy`
- **Lokasi:** `grails-app/domain/org/pih/warehouse/order/OrderItemDetails.groovy`
- **Fungsi file:** Detail agregat item order (view wrapper)
- **Field:**
  | Field | Line (baru) | Tipe Lama | Tipe Baru |
  |---|---|---|---|
  | `quantity` | 12 | `Integer` | `BigDecimal` |

### 19. `OrderItemSummary.groovy`
- **Lokasi:** `grails-app/domain/org/pih/warehouse/order/OrderItemSummary.groovy`
- **Fungsi file:** Ringkasan item order untuk reporting
- **Field diubah:**
  | Field | Line (baru) | Tipe Lama | Tipe Baru |
  |---|---|---|---|
  | `quantity` | 12 | `Integer` | `BigDecimal` |
  | `quantityOrdered` | 18 | `Integer` | `BigDecimal` |
  | `quantityShipped` | 19 | `Integer` | `BigDecimal` |
  | `quantityReceived` | 20 | `Integer` | `BigDecimal` |
  | `quantityCanceled` | 21 | `Integer` | `BigDecimal` |
  | `quantityInvoiced` | 22 | `Integer` | `BigDecimal` |

### 20. `OrderSummary.groovy`
- **Lokasi:** `grails-app/domain/org/pih/warehouse/order/OrderSummary.groovy`
- **Fungsi file:** Ringkasan order untuk reporting
- **Field diubah:**
  | Field | Line (baru) | Tipe Lama | Tipe Baru |
  |---|---|---|---|
  | `quantityOrdered` | 17 | `Integer` | `BigDecimal` |
  | `quantityShipped` | 18 | `Integer` | `BigDecimal` |
  | `quantityReceived` | 19 | `Integer` | `BigDecimal` |
  | `quantityCanceled` | 20 | `Integer` | `BigDecimal` |
  | `quantityInvoiced` | 21 | `Integer` | `BigDecimal` |

### 21. `PicklistItem.groovy`
- **Lokasi:** `grails-app/domain/org/pih/warehouse/picklist/PicklistItem.groovy`
- **Fungsi file:** Item picklist — picking barang
- **Field diubah:**
  | Field | Line (baru) | Tipe Lama | Tipe Baru |
  |---|---|---|---|
  | `quantityPicked` | 44 | `Integer` | `BigDecimal` |
  | `quantity` | 48 | `Integer` | `BigDecimal` |

### 22. `ProductAvailability.groovy`
- **Lokasi:** `grails-app/domain/org/pih/warehouse/product/ProductAvailability.groovy`
- **Fungsi file:** Availability product — stok real-time per location
- **Field diubah:**
  | Field | Line (baru) | Tipe Lama | Tipe Baru |
  |---|---|---|---|
  | `quantityOnHand` | 31 | `Integer` | `BigDecimal` |
  | `quantityAllocated` | 32 | `Integer` | `BigDecimal` |
  | `quantityOnHold` | 33 | `Integer` | `BigDecimal` |
  | `quantityAvailableToPromise` | 34 | `Integer` | `BigDecimal` |
  | `quantityNotPicked` | 35 | `Integer` | `BigDecimal` |

### 23. `ProductPackage.groovy`
- **Lokasi:** `grails-app/domain/org/pih/warehouse/product/ProductPackage.groovy`
- **Fungsi file:** Package/kemasan produk — jumlah unit per box
- **Field:**
  | Field | Line (baru) | Tipe Lama | Tipe Baru |
  |---|---|---|---|
  | `quantity` | 36 | `Integer` | `BigDecimal` |

### 24. `Product.groovy`
- **Lokasi:** `grails-app/domain/org/pih/warehouse/product/Product.groovy`
- **Fungsi file:** Entitas produk utama
- **Method diubah:**
  | Method | Line (baru) | Tipe Lama | Tipe Baru |
  |---|---|---|---|
  | `getStatus(locationId, currentQuantity)` param | 527 | `Integer` | `BigDecimal` |

### 25. `ProductSupplier.groovy`
- **Lokasi:** `grails-app/domain/org/pih/warehouse/product/ProductSupplier.groovy`
- **Fungsi file:** Supplier produk — harga pengadaan
- **Field diubah:**
  | Field | Line (baru) | Tipe Lama | Tipe Baru |
  |---|---|---|---|
  | `defaultPackageQuantity` (local var) | 195 | `Integer` | `BigDecimal` |

### 26. `ReceiptItem.groovy`
- **Lokasi:** `grails-app/domain/org/pih/warehouse/receiving/ReceiptItem.groovy`
- **Fungsi file:** Item penerimaan barang
- **Field diubah:**
  | Field | Line (baru) | Tipe Lama | Tipe Baru |
  |---|---|---|---|
  | `quantityShipped` | 31 | `Integer` | `BigDecimal` |
  | `quantityReceived` | 32 | `Integer` | `BigDecimal` |
  | `quantityCanceled` | 33 | `Integer` | `BigDecimal` |

### 27. `RequisitionItem.groovy`
- **Lokasi:** `grails-app/domain/org/pih/warehouse/requisition/RequisitionItem.groovy`
- **Fungsi file:** Item requisition — permintaan barang internal
- **Field/method diubah:**
  | Field/Method | Line (baru) | Tipe Lama | Tipe Baru |
  |---|---|---|---|
  | `quantity` | 49 | `Integer` | `BigDecimal` |
  | `quantityCounted` | 52 | `Integer` | `BigDecimal` |
  | `quantityApproved` | 58 | `Integer` | `BigDecimal` |
  | `quantityCanceled` | 59 | `Integer` | `BigDecimal` |
  | `changeQuantity(newQuantity, ...)` param | 286 | `Integer` | `BigDecimal` |
  | `changeQuantity(newQuantity, pkg, ...)` param | 298 | `Integer` | `BigDecimal` |
  | `chooseSubstitute(..., newQuantity, ...)` param | 339 | `Integer` | `BigDecimal` |
  | `getQuantityIssued()` | 679 | `Integer` → `BigDecimal` | return type |
  | `getQuantityAdjusted()` | 684 | `Integer` → `BigDecimal` | return type |

### 28. `ShipmentItem.groovy`
- **Lokasi:** `grails-app/domain/org/pih/warehouse/shipping/ShipmentItem.groovy`
- **Fungsi file:** Item pengiriman barang
- **Field/method diubah:**
  | Field/Method | Line (baru) | Tipe Lama | Tipe Baru |
  |---|---|---|---|
  | `quantity` | 34 | `Integer` | `BigDecimal` |
  | `quantityReceived()` | 185 | `Integer` → `BigDecimal` | return type |
  | `getQuantityReceived()` | 189 | `Integer` → `BigDecimal` | return type |
  | `quantityCanceled()` | 200 | `Integer` → `BigDecimal` | return type |
  | `getQuantityCanceled()` | 204 | `Integer` → `BigDecimal` | return type |
  | `getQuantityRemaining()` | 211 | `Integer` → `BigDecimal` | return type |
  | `getQuantityReceivedAndCanceled()` | 215 | `Integer` → `BigDecimal` | return type |
  | `getQuantityRemainingToShip()` | 219 | `Integer` → `BigDecimal` | return type |
  | `getQuantityPicked()` | 228 | `Integer` → `BigDecimal` | return type + local var `quantityPicked` |
  | `getUnavailableQuantityPicked()` | 238 | `Integer` → `BigDecimal` | return type + local var |
  | `getQuantityPickedFromOrders()` | 254 | `Integer` → `BigDecimal` | return type + local var |
  | `getQuantityInvoiced()` | 278 | `Integer` → `BigDecimal` | return type |
  | `getQuantityInvoicedInStandardUom()` | 282 | `Integer` → `BigDecimal` | return type |
  | `getQuantityToInvoiceInStandardUom()` | 286 | `Integer` → `BigDecimal` | return type |
  | `getQuantityToInvoice()` | 291 | `Integer` → `BigDecimal` | return type |

### 29. `Shipment.groovy`
- **Lokasi:** `grails-app/domain/org/pih/warehouse/shipping/Shipment.groovy`
- **Fungsi file:** Entitas pengiriman utama
- **Method diubah:**
  | Method | Line (baru) | Tipe Lama | Tipe Baru |
  |---|---|---|---|
  | `cloneContainer(Container, quantity)` param | 516 | `Integer` | `BigDecimal` |

### 30. Reporting domain files
- `ConsumptionFact.groovy` (line 26): `quantity = BigDecimal.ZERO` (was `0.0`)
- `CycleCountProductSummary.groovy` (line 12): `quantityVariance` → `BigDecimal`
- `InventoryShrinkageResult.groovy` (line 9): `quantitySum` → `BigDecimal`
- `TransactionFact.groovy` (line 27): `quantity = BigDecimal.ZERO` (was `0.0`)

---

## B. Controllers (6 files)

### 1. `CombinedShipmentItemApiController.groovy`
- **Lokasi:** `grails-app/controllers/org/pih/warehouse/api/CombinedShipmentItemApiController.groovy`
- **Fungsi file:** API untuk combined shipment items
- **Method:** anonymous block di action
  | Lokasi (line baru) | Perubahan | Alasan |
  |---|---|---|
  | 168 | `Integer quantityPerUom = orderItem?.quantityPerUom?.toInteger()` → `def quantityPerUom = orderItem?.quantityPerUom` | `.quantityPerUom` skrg `BigDecimal` |

### 2. `PartialReceivingApiController.groovy`
- **Lokasi:** `grails-app/controllers/org/pih/warehouse/api/PartialReceivingApiController.groovy`
- **Fungsi file:** API untuk partial receiving
- **Method:** action block
  | Lokasi (line baru) | Perubahan | Alasan |
  |---|---|---|
  | 114 | `tokens[11].toInteger()` → `tokens[11].toBigDecimal()` | quantity sekarang decimal |

### 3. `InventoryItemController.groovy`
- **Lokasi:** `grails-app/controllers/org/pih/warehouse/inventory/InventoryItemController.groovy`
- **Fungsi file:** Controller item inventory
- **Method:** action block
  | Lokasi (line baru) | Perubahan | Alasan |
  |---|---|---|
  | 351 | `.toInteger()` → `.toBigDecimal()` pada `quantityPurchased` | hasil kalkulasi decimal |
  | 357 | `.toInteger()` → `.toBigDecimal()` pada accumulator | hasil kalkulasi decimal |

### 4. `OrderController.groovy`
- **Lokasi:** `grails-app/controllers/org/pih/warehouse/order/OrderController.groovy`
- **Fungsi file:** Controller order/purchase order
- **Method:** action block
  | Lokasi (line baru) | Perubahan | Alasan |
  |---|---|---|
  | 772 | `params.quantity as Integer` → `params.quantity as BigDecimal` | orderItem.quantity sekarang decimal |

### 5. `ProductAssociationController.groovy`
- **Lokasi:** `grails-app/controllers/org/pih/warehouse/product/ProductAssociationController.groovy`
- **Fungsi file:** Controller asosiasi produk (substitusi, similar)
- **Method:** action block
  | Lokasi (line baru) | Perubahan | Alasan |
  |---|---|---|
  | 259 | `params.quantity as Integer` → `params.quantity as BigDecimal` | quantity sekarang decimal |

### 6. `CreateShipmentWorkflowController.groovy`
- **Lokasi:** `grails-app/controllers/org/pih/warehouse/shipping/CreateShipmentWorkflowController.groovy`
- **Fungsi file:** Controller workflow pembuatan shipment
- **Method:** action block
  | Lokasi (line baru) | Perubahan | Alasan |
  |---|---|---|
  | 1033 | `flash.cloneQuantity as Integer` → `flash.cloneQuantity as BigDecimal` | param method copyContainer |
  | 1460 | `quantityFromForm as Integer` → `quantityFromForm as BigDecimal` | quantity sekarang decimal |

---

## C. Services (7 files)

### 1. `OrderService.groovy`
- **Lokasi:** `grails-app/services/org/pih/warehouse/order/OrderService.groovy`
- **Fungsi file:** Business logic order/purchase order
- **Method:** block di `saveOrder` / `createOrder`
  | Lokasi (line baru) | Perubahan | Alasan |
  |---|---|---|
  | 635 | `quantityPerUom as Integer` → `quantityPerUom as BigDecimal` | display name hanya format string |
  | 637 | `quantityPerUom as Integer` → `quantityPerUom` (cast dihapus) | productPackage.quantity skrg BigDecimal |

### 2. `InventoryImportDataService.groovy`
- **Lokasi:** `grails-app/services/org/pih/warehouse/importer/InventoryImportDataService.groovy`
- **Fungsi file:** Import data inventory dari CSV/Excel
- **Method:** `importData` / row processing
  | Lokasi (line baru) | Perubahan | Alasan |
  |---|---|---|
  | 419 | `Integer quantityToImport = entry['quantity'] as Integer` → `def quantityToImport = entry['quantity']` | quantity sekarang BigDecimal |
  | 448 | `.toInteger() as Integer` → `.toBigDecimal()` | parsing decimal |

### 3. `ProductPackageImportDataService.groovy`
- **Lokasi:** `grails-app/services/org/pih/warehouse/importer/ProductPackageImportDataService.groovy`
- **Fungsi file:** Import product package dari file eksternal
- **Method:** `matchProductPackage`
  | Lokasi (line baru) | Perubahan | Alasan |
  |---|---|---|
  | 55 | `quantity.toInteger()` → `quantity.toBigDecimal()` | quantity sekarang decimal |

### 4. `PrepaymentInvoiceService.groovy`
- **Lokasi:** `grails-app/services/org/pih/warehouse/invoice/PrepaymentInvoiceService.groovy`
- **Fungsi file:** Business logic prepayment invoice
- **Method:** block di method
  | Lokasi (line baru) | Perubahan | Alasan |
  |---|---|---|
  | 310 | `Integer quantity = properties.quantity as Integer` → `def quantity = properties.quantity` | quantity sekarang BigDecimal |

### 5. `ProductMergeService.groovy`
- **Lokasi:** `grails-app/services/org/pih/warehouse/product/ProductMergeService.groovy`
- **Fungsi file:** Service merge product (gabung 2 produk)
- **Method:** `mergeProducts`
  | Lokasi (line baru) | Perubahan | Alasan |
  |---|---|---|
  | 231 | `it.quantityOnHand as Integer` → `it.quantityOnHand as BigDecimal` | quantityOnHand skrg decimal |
  | 246 | (same pattern) | |

### 6. `ReportService.groovy`
- **Lokasi:** `grails-app/services/org/pih/warehouse/report/ReportService.groovy`
- **Fungsi file:** Generate laporan
- **Method:** block di laporan
  | Lokasi (line baru) | Perubahan | Alasan |
  |---|---|---|
  | 772 | `it.quantityRemaining.toInteger()` → `it.quantityRemaining.toBigDecimal()` | quantityRemaining skrg decimal |

### 7. `LoadDataService.groovy`
- **Lokasi:** `grails-app/services/org/pih/warehouse/data/LoadDataService.groovy`
- **Fungsi file:** Load/seed data awal
- **Method:** row processing
  | Lokasi (line baru) | Perubahan | Alasan |
  |---|---|---|
  | 201 | `.toInteger()` → `.toBigDecimal()` | package quantity skrg decimal |

---

## D. Database Migration (Liquibase)

### `changelog-2026-05-26-1600-change-integer-quantity-to-decimal.xml`
- **Lokasi:** `grails-app/migrations/0.9.x/changelog-2026-05-26-1600-change-integer-quantity-to-decimal.xml`
- **Fungsi:** Migrasi DB — ubah `int(11)` → `decimal(19,3)` untuk 28+ kolom
- **Catatan:** Awalnya pakai `<modifyDataType>` tapi tidak support di Liquibase 1.9 XSD. Diganti ke `<modifyColumn>`.
- **Tabel yang diubah:**

| Tabel | Kolom |
|---|---|
| `order_item` | `quantity` |
| `shipment_item` | `quantity` |
| `fulfillment_item` | `quantity` |
| `receipt_item` | `quantity_shipped`, `quantity_received`, `quantity_canceled` |
| `inventory_snapshot` | `quantity_on_hand`, `quantity_inbound`, `quantity_outbound` |
| `inventory_item_snapshot` | `quantity_on_hand`, `quantity_inbound`, `quantity_outbound`, `quantity_available_to_promise` |
| `transaction_entry` | `quantity` |
| `invoice_item` | `quantity` |
| `requisition_item` | `quantity`, `quantity_counted`, `quantity_approved`, `quantity_canceled` |
| `product_package` | `quantity` |
| `product_availability` | `quantity_on_hand`, `quantity_allocated`, `quantity_on_hold`, `quantity_available_to_promise` |
| `picklist_item` | `quantity`, `quantity_picked` |
| `cycle_count_item` | `quantity_on_hand`, `quantity_counted` |

---

## E. DB Views (tidak perlu diubah)

Tabel-tabel berikut adalah VIEW (bukan BASE TABLE), kolomnya akan otomatis mengikuti perubahan tabel dasarnya:
- `order_item_details`
- `invoice_item_candidate`
- `cycle_count_candidate`
- `cycle_count_details`
- `cycle_count_summary`
- `pending_cycle_count_request`
- `requirement`
- `inventory_audit_details`
- `inventory_audit_summary`

---

## F. Constraint Fixes (commit `cd4ec722d`)

Setelah domain class diubah jadi `BigDecimal`, Grails validation `min:` dan `range:` yang tadinya pakai `Integer` literal perlu disesuaikan.

### Files yang diubah:

| File | Line | Sebelum | Sesudah | Alasan |
|---|---|---|---|---|
| `OrderItem.groovy` | 144 | `quantity(min: 1)` | `quantity(min: 1.0)` | Constraint value harus kompatibel dengan BigDecimal |
| `InvoiceItem.groovy` | 83 | `quantity(min: 0)` | `quantity(min: 0.0)` | Sama |
| `RequisitionItem.groovy` | 133 | `quantity(min: 0)` | `quantity(min: 0.0)` | Sama |
| `ShipmentItem.groovy` | 94 | `quantity(min: 0, range: 0..2147483646)` | `quantity(min: 0.0, range: 0.0..2147483646.0)` | Range BigDecimal literal |
| `ReceiptItem.groovy` | 61 | `quantityShipped(range: 0..2147483646)` | `quantityShipped(range: 0.0..2147483646.0)` | Sama |
| `InventoryLevel.groovy` | 97-100 | `range: 0..2147483646` | `range: 0.0..2147483646.0` | Sama untuk minQuantity, reorderQuantity, maxQuantity, forecastQuantity |

---

## G. Build & Deployment

### Build WAR
```bash
# Build executable WAR (Spring Boot repackage)
./gradlew bootRepackage -x generateGitProperties --no-daemon

# Skip webpack untuk build lebih cepat (hanya backend)
./gradlew bootRepackage -x generateGitProperties -x npm_run_bundle --no-daemon
```

### Build Docker Image
```bash
cp build/libs/openboxes.war docker/
docker build -t wms-openboxes:custom docker/ -f docker/Dockerfile
```

### docker-compose.yml
```yaml
services:
  app:
    image: wms-openboxes:custom   # ganti ghcr.io/openboxes/openboxes:latest
    ...
```

### Catatan Docker
- Base image: `eclipse-temurin:8-jre-jammy`
- Entrypoint: `java -Dgrails.env=prod -jar /app/openboxes.war`
- Harus pakai `bootRepackage` biar WAR executable (punya Main-Class: WarLauncher)

---

## Catatan Penting

1. **Kompilasi:** `./gradlew compileGroovy` — ✅ SUCCESS (0 error)
2. **Docker custom image:** ✅ Sudah build dan running (`wms-openboxes:custom`)
3. **DB columns:** Semua kolom quantity diubah manual via ALTER ke `decimal(19,3)` ✅
4. **Constraint validation:** `min:` dan `range:` pake BigDecimal literal (`1.0`, `0.0`) biar kompatibel dengan property type ✅
5. **Testing:** Login + dashboard + PO list — ✅ OK. Input decimal 12.233 perlu dicek di UI.

---

## H. GSP View Fixes — Decimal Display Format (PR #2)

> **Issue:** [#2 Packing List & shipment views display quantity without decimal places](https://github.com/agusprp/wms-openboxes/issues/2)
> **Branch:** `bugfix/packing-list-decimal-display`
> **Base:** `feature/int-to-decimal-quantity`

### Latar Belakang

Setelah domain fields berubah dari `Integer` ke `BigDecimal` dan DB columns ke `decimal(19,3)`, GSP views masih menggunakan format number `###,##0` yang hanya menampilkan bilangan bulat. Contoh: `12.5` kg muncul sebagai `12`.

### File & Perubahan

| File | Lines | Sebelum | Sesudah |
|---|---|---|---|
| `grails-app/views/stockMovement/_packingList.gsp` | 164, 168, 171 | `###,##0` | `###,##0.###` |
| `grails-app/views/shipment/showDetails.gsp` | 448, 453, 456 | `###,##0` | `###,##0.###` |
| `grails-app/views/email/_shipmentItemReceived.gsp` | 86, 89 | `###,##0` | `###,##0.###` |
| `grails-app/views/email/_shipmentItemShipped.gsp` | 72 | `###,##0` | `###,##0.###` |
| `grails-app/views/email/_shipmentReceived.gsp` | 285, 288 | `###,##0` | `###,##0.###` |
| `grails-app/views/email/_shipmentShipped.gsp` | 298 | `###,##0` | `###,##0.###` |

### Detail Perubahan

#### 1. `stockMovement/_packingList.gsp`

Halaman Packing List di Stock Movement show — yang Bro laporkan.

**Sebelum:**
```html
<g:formatNumber number="${shipmentItem?.quantity}" format="###,##0" />
<g:formatNumber number="${shipmentItem?.quantityReceived()}" format="###,##0"/>
<g:formatNumber number="${shipmentItem?.quantityCanceled()}" format="###,##0"/>
```

**Sesudah:**
```html
<g:formatNumber number="${shipmentItem?.quantity}" format="###,##0.###" />
<g:formatNumber number="${shipmentItem?.quantityReceived()}" format="###,##0.###"/>
<g:formatNumber number="${shipmentItem?.quantityCanceled()}" format="###,##0.###"/>
```

#### 2. `shipment/showDetails.gsp`

**Sebelum:**
```html
<g:formatNumber number="${shipmentItem?.quantity}" format="###,##0" />
<g:formatNumber number="${shipmentItem?.quantityReceived()}" format="###,##0"/>
<g:formatNumber number="${shipmentItem?.quantityCanceled()}" format="###,##0"/>
```

**Sesudah:**
```html
<g:formatNumber number="${shipmentItem?.quantity}" format="###,##0.###" />
<g:formatNumber number="${shipmentItem?.quantityReceived()}" format="###,##0.###"/>
<g:formatNumber number="${shipmentItem?.quantityCanceled()}" format="###,##0.###"/>
```

#### 3. Email Templates (`email/_shipmentItemReceived`, `_shipmentItemShipped`, `_shipmentReceived`, `_shipmentShipped`)

Semua format `###,##0` untuk field quantity diubah ke `###,##0.###` — total 6 lokasi di 4 file.

### Tidak diubah (masih integer display — sengaja)

Format `###,##0.00`, `###,##0.00##`, `###,##0.####` untuk **price/totalValue** fields tetap dipertahankan — karena price fields pakai format 2-4 decimal yang sudah sesuai.

### Catatan

1. **Format `###,##0.###`:** Menampilkan min 0 dan maks 3 angka decimal. Contoh: `0` → `0`, `0.5` → `0.5`, `1.234` → `1.234`.
2. **Build ulang diperlukan:** Perubahan hanya di GSP, cukup rebuild WAR + redeploy. Tidak perlu migrasi DB.

---

## I. Service & Command Fixes — Integer → BigDecimal Return Types (PR #3)

> **Issue:** [#1](https://github.com/agusprp/wms-openboxes/issues/1) (follow-up) — StockCard throw ClassCastException
> **Branch:** `bugfix/packing-list-decimal-display` (merged to `feature/int-to-decimal-quantity`)

### Latar Belakang

Setelah domain fields berubah dari `Integer` ke `BigDecimal`, beberapa Command class dan Service method masih mendeklarasikan tipe return `Integer`. Akibatnya halaman Stock Card (`showStockCard`) throw `ClassCastException: java.lang.Integer cannot be cast to java.math.BigDecimal`.

### Perubahan

#### 1. `StockCardCommand.groovy`

| Field | Sebelum | Sesudah |
|---|---|---|
| `totalQuantity` | `Integer` | `BigDecimal` |
| `totalQuantityAvailableToPromise` | `Integer` | `BigDecimal` |
| `quantityByInventoryItemMap` | `Map<InventoryItem, Integer>` | `Map<InventoryItem, BigDecimal>` |

#### 2. `InventoryService.groovy` — Method Signatures

11 method signatures diubah dari `Integer` ke `BigDecimal`:

| Method | Perubahan |
|---|---|
| `getQuantity(Location, Product, String)` | `Integer` → `BigDecimal` |
| `getQuantityAvailableToPromise(Location, Product)` | `Integer` → `BigDecimal` |
| `getQuantityOnHand(Location, Product)` | `Integer` → `BigDecimal` |
| `getQuantityToReceive(Location, Product)` | `Integer` → `BigDecimal` |
| `getQuantityToShip(Location, Product)` | `Integer` → `BigDecimal` |
| `getQuantityFromBinLocation(Location, Location, InventoryItem)` | `Integer` → `BigDecimal` |
| `getQuantity(Inventory, InventoryItem)` | `Integer` → `BigDecimal` |
| `getQuantity(Inventory, Location, InventoryItem)` | `Integer` → `BigDecimal` |
| `getQuantityAvailableToPromise(InventoryItem)` | `Integer` → `BigDecimal` |
| `getQuantityAvailableToPromise(Inventory, InventoryItem)` | `Integer` → `BigDecimal` |
| `getQuantityAvailableToPromise(Product, Location)` | `Integer` → `BigDecimal` |
| `getQuantityByProductMap(List<TransactionEntry>)` | `Map<Product, Integer>` → `Map<Product, BigDecimal>` |
| `getQuantityByProductMap(String)` | `Map<Product, Integer>` → `Map<Product, BigDecimal>` |
| `getQuantityByProductMap(Location)` | `Map<Product, Integer>` → `Map<Product, BigDecimal>` |
| `getQuantityByInventoryItemMap(List<TransactionEntry>)` | `Map<InventoryItem, Integer>` → `Map<InventoryItem, BigDecimal>` |
| `getQuantityByProductAndInventoryItemMap(List<TransactionEntry>)` | `Map<Product, Map<InventoryItem, Integer>>` → `Map<Product, Map<InventoryItem, BigDecimal>>` |
| Local variable `quantity` (line 1159) | `Integer quantity` → `def quantity` |
| Local variable `quantityAvailable` (line 1275) | `Integer quantityAvailable` → `def quantityAvailable` |
| Local variables `totalNewQty`, `existingOldQty` (line 1392-1393) | `Integer` → `def` |
| Local variable `quantityOnHand` (line 1975) | `Integer quantityOnHand` → `def quantityOnHand` |
| `reorderProductsQuantityMap`, `minimumProductsQuantityMap` | `Map<Product, Integer>` → `Map<Product, BigDecimal>` |
| Local variable `totalQuantityLostToExpiry` (line 3429) | `Integer` → `def` |

### 3. GSP Format Fixes (Inventory Item Views)

| File | Lines | Format |
|---|---|---|
| `inventoryItem/_productDetails.gsp` | 23, 37, 53, 95, 113, 125, 141 | `###,###,###` → `###,###,###.###` |
| `inventoryItem/_showCurrentStock.gsp` | 68, 72, 110, 129 | `###,###,###` → `###,###,###.###` |
| `inventoryItem/_showProductAssociations.gsp` | 42, 75 | `###,###,###` → `###,###,###.###` |

### Catatan

1. **Tidak ada perubahan logika bisnis** — hanya tipe deklarasi yang disesuaikan.
2. **Groovy handling:** Local variables pakai `def` agar tipe otomatis mengikuti nilai yang diberikan.
3. **Map types:** Value type di map generics disesuaikan dari `Integer` ke `BigDecimal`.
4. **Format display:** 3 digit decimal untuk quantity, sesuai DB precision `decimal(19,3)`.
