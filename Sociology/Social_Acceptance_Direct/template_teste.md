FIELD text TYPE QUOTATION
    SCOPE ITEM
    DESCRIPTION Citação literal que serve como prova documental da relação causal.
    GUIDELINES
        Extraia a evidência textual MÍNIMA e SUFICIENTE que suporte o link causal.
        A citação deve conter obrigatoriamente um indicador de influência (verbo de ação ou partícula conectiva).
        REGRA DE OURO: Se o texto não afirma explicitamente que A afeta B, NÃO extraia.
        Evite inferências; se o autor diz "associação", extraia como associação, não causa.
        
        Exemplo: "O aumento do custo reduziu a aceitação pública."
        Verbatim: "custo reduziu a aceitação pública"
    END GUIDELINES
END FIELD

FIELD note TYPE MEMO
    SCOPE ITEM
    DESCRIPTION Descrição do mecanismo lógico/teórico que conecta os fatores na cadeia.
    GUIDELINES
        Explique o caminho lógico revelado pela evidência.
        Não resuma o texto; descreva a dinâmica de causa e efeito.
        
        ESTRUTURA PARA RAG: "O fator [A] impacta [B] através de [Mecanismo], resultando em [C]."
        
        Exemplo: "A transparência no processo reduz a percepção de risco, o que por sua vez estabiliza a aceitação a longo prazo."
    END GUIDELINES
END FIELD

FIELD chain TYPE CHAIN
    SCOPE ITEM
    ARITY >= 2
    DESCRIPTION Cadeia causal indiferenciada (A -> B) focada em acurácia direcional.
    GUIDELINES
        FOCO TOTAL NA DIREÇÃO: Cause/Enabler/Prerequisite -> Outcome/Effect/Result.
        
        TESTE DE ACURÁCIA:
        1. Identifique o fator que inicia a ação (influence factor).
        2. Identifique o fator que sofre a alteração (consequence factor).
        3. Conecte com o símbolo "->"
        
        DIREÇÃO LINGUÍSTICA (Inversão):
        Siga rigorosamente a lógica de dependência:
        "B depende de A" ou "B requer A" -> Mapear como A -> B.
        "A causa B" ou "A permite B" -> Mapear como A -> B.

        CADEIAS COMPLETAS (Caminhos):
        Mantenha cadeias sequenciais longas (A -> B -> C -> D) no mesmo bloco ITEM.

        CONTROLE DE GRANULARIDADE (Stakeholder Measurability):
        Use termos que um cidadão comum possa perceber (ex: 'Custo', 'Confiança').
        Normalize variações (ex: 'Trust_in_government' e 'Institutional_trust' -> Trust).
    END GUIDELINES
END FIELD
