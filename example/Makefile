LEVEL= ..
PROG=prog
ABC_RT_LIB=$(LEVEL)/Debug+Asserts/lib/libcall_site_tracing_rt.a
CGC_RT_LIB=$(LEVEL)/Debug+Asserts/lib/libcall_edge_tracing_rt.a
CGC_LLVM_LIB=$(LEVEL)/Debug+Asserts/lib/LLVMCallEdgeTracing.so
ABC_LLVM_LIB=$(LEVEL)/Debug+Asserts/lib/LLVMCallSiteTracing.so
REORDER_LIB=$(LEVEL)/Debug+Asserts/lib/LLVMFunctionReordering.so
ICC_LLVM_LIB=$(LEVEL)/Debug+Asserts/lib/LLVMCacheCounting.so
ICC_RT_LIB=$(LEVEL)/Debug+Asserts/lib/libcache_counting_rt.a
LIBPAPI=$(HOME)/usr/local/lib/libpapi.a
BITCODED = $(PROG).bc
COMPILED = $(PROG).out
ABC_TR	= $(PROG).abc_tr.out
ABC_OUT	= $(PROG).abc.out
BABC_OUT = $(PROG).babc.out
ICC_OUT = $(PROG).icc.out

CGC_OUT = $(PROG).cgc.out
CGC_TR = $(PROG).cgc_tr.out


$(BABC_OUT): $(BITCODED)
	opt -load $(BABC_LLVM_LIB) 

$(ICC_OUT): $(BITCODED)
	opt -load $(ICC_LLVM_LIB) -add-inst-cache-counters $< <$(BITCODED)> $@.bc
	llc $@.bc -o $@.s
	gcc $@.s $(ICC_RT_LIB) $(LIBPAPI) -o $@ 

$(CGC_OUT): layout.cgc $(BITCODED)
	opt -load $(REORDER_LIB) -rlavaee-reorder-functions -layout $< <$(BITCODED)> $@.bc
	llc $@.bc -o $@.s
	gcc $@.s -o $@

$(CGC_TR): $(BITCODED) $(CGC_RT_LIB)
	opt -load $(CGC_LLVM_LIB) -trace-call-edges $< -o $@.bc
	llc $@.bc -o $@.s
	clang++ $@.s $(CGC_RT_LIB) -o $@ -lpthread

$(ABC_OUT): layout.abc $(BITCODED)
	opt -load $(REORDER_LIB) -rlavaee-reorder-functions -layout $< <$(BITCODED)> $@.bc
	llc $@.bc -o $@.s
	gcc $@.s -o $@

$(ABC_TR): $(BITCODED) $(ABC_RT_LIB)
	opt -load $(ABC_LLVM_LIB) -trace-call-sites $< -o $@.bc
	llc $@.bc -o $@.s
	clang++ $@.s $(ABC_RT_LIB) -o $@ -lpthread

$(BITCODED): test.c test_util.c
	clang -emit-llvm test.c -o test.bc -c -O3
	clang -emit-llvm test_util.c -o test_util.bc -c -O3
	llvm-link test.bc test_util.bc -o $@

$(COMPILED): $(BITCODED)
	llc $< -o $(PROG).s
	clang $(PROG).s -o $@
 

